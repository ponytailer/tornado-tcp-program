
## 2.2 ioloop开启时的回调函数

###### 1.IOLoop.add_callback(callback, *args, **kwargs)
这个函数是最简单的，在ioloop开启后执行的回调函数callback，*args和**kwargs都是这个回调函数的参数。一般我们的server都是单进程单线程的，即使是多线程，那么这个函数也是安全的。

##### 2.IOLoop.add_callback_from_signal(callback, *args, **kwargs)
这个函数和上面的很类似，只不过他是在without any stack_context的时候去执行，关于stack_context，查阅[这里](http://www.tornadoweb.org/en/stable/stack_context.html#module-tornado.stack_context)。

##### 3.IOLoop.add_future(future, callback)
这个函数呢也是添加一个callback函数，当给定的这个future执行完的时候，callback会去执行，这个函数有唯一的一个参数就是这个future对象。关于future呢，后面会详细去讲。

##### 4.IOLoop.add_timeout(deadline, callback, *args, **kwargs)
执行callback函数在deadline的时候，这个deadline可以是time.time，也可以是datetime.timedelta。还有，这个函数线程不安全。

##### 5.IOLoop.call_at(when, callback, *args, **kwargs)
这个函数我们用的就很多了，在ioloop启动后，会在when这个时间点去执行callback函数。类似一个定时器的功能。

##### 6.IOLoop.call_later(delay, callback, *args, **kwargs)
这个函数和上面的差不多，这是在ioloop启动后的delay秒后，去执行callback。上面的是一个时间点，而这个函数是多少秒。
```ioloop.IOLoop.current().call_later(3, func)```这就是在ioloop启动的3s以后来执行func函数。

##### 7.IOLoop.remove_timeout(timeout)
这个函数就是用来移除上面通过add_timeout注册的callback函数。

##### 8.IOLoop.spawn_callback(callback, *args, **kwargs)
这个函数也是去执行一个回调函数，但是和上面说过的其他callback不同，它和回调者的栈上下文没有关联，因此呢，他比较时候去做一些独立的功能回调。

##### 9.IOLoop.time()
它返回的时候ioloop开启后的时间，返回的值跟time.time差不多。