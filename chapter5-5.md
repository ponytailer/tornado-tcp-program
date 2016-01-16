##ioloop分析

######咱们先来简单说一下ioloop的源码
```
while True:
    poll_timeout = 3600.0
    
    # Prevent IO event starvation by delaying new callbacks
    # to the next iteration of the event loop.
    with self._callback_lock:
    	callbacks = self._callbacks
    	self._callbacks = []
    for callback in callbacks:
    	self._run_callback(callback)
    
    if self._timeouts:
    	now = self.time()
    	while self._timeouts:
    		if self._timeouts[0].callback is None:
    			# the timeout was cancelled
    			heapq.heappop(self._timeouts)
    		elif self._timeouts[0].deadline <= now:
    			timeout = heapq.heappop(self._timeouts)
    			self._run_callback(timeout.callback)
    		else:
    			seconds = self._timeouts[0].deadline - now
    			poll_timeout = min(seconds, poll_timeout)
    			break
    
    if self._callbacks:
    	# If any callbacks or timeouts called add_callback,
    	# we don't want to wait in poll() before we run them.
    	poll_timeout = 0.0
    
    if not self._running:
    	break
    
    if self._blocking_signal_threshold is not None:
    	# clear alarm so it doesn't fire while poll is waiting for
    	# events.
    	signal.setitimer(signal.ITIMER_REAL, 0, 0)
    
    try:
    	event_pairs = self._impl.poll(poll_timeout)
    except Exception as e:
    	# Depending on python version and IOLoop implementation,
    	# different exception types may be thrown and there are
    	# two ways EINTR might be signaled:
    	# * e.errno == errno.EINTR
    	# * e.args is like (errno.EINTR, 'Interrupted system call')
    	if (getattr(e, 'errno', None) == errno.EINTR or
    		(isinstance(getattr(e, 'args', None), tuple) and
    		 len(e.args) == 2 and e.args[0] == errno.EINTR)):
    		continue
    	else:
    		raise
    
    if self._blocking_signal_threshold is not None:
    	signal.setitimer(signal.ITIMER_REAL,
    					 self._blocking_signal_threshold, 0)
    
    
    self._events.update(event_pairs)
    while self._events:
    	fd, events = self._events.popitem()
    	try:
    		self._handlers[fd](fd, events)
    	except (OSError, IOError) as e:
    		if e.args[0] == errno.EPIPE:
    			# Happens when the client closes the connection
    			pass
    		else:
    			app_log.error("Exception in I/O handler for fd %s",
    						  fd, exc_info=True)
    	except Exception:
    		app_log.error("Exception in I/O handler for fd %s",
    					  fd, exc_info=True)
    					  ```