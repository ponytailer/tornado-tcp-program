## epoll

我们知道tornado通过非阻塞的方式以及对epoll的运用，才使得性能上得到了很大的提升。那么epoll到底是什么，它在tornado中扮演着怎样的角色呢？

1.epoll解读

说到epoll，就得先说说阻塞和非阻塞，这里大家自行百度或者脑补。
我们通常处理数据流可能是这样的
```
While true：
    for i in stream:
        if i has data:
            Do something with i
```

这种方式显然很差劲，他会一直轮巡，不管数据流中是否有IO事件。针对这种情况就出现了select，
它可以甄别数据流是否有IO，当无IO的时候，就阻塞在那里，直到下一次IO发生，在进行操作。
```
While true：
    select（stream）
    for i in stream：
        if i has data：
            Do something
```

虽然我们不用白白的轮询，也知道了是否发生IO，，但却并不知道是那几个流（可能有一个，多个，甚至全部），我们只能无差别轮询所有流，找出能读出数据，或者写入数据的流，对他们进行操作。
因此epoll就诞生了，epoll全称就是event poll，和咱们平常用的轮训不同，基于事件的epoll会把哪个数据流发生了怎样的事件告诉我们。
```
while true 
	active_stream[] = epoll_wait(epollfd)
	for i in active_stream[] 
		read or write till unavailable
	
```