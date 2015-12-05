
## 2.2 ioloop开启时的回调函数

###### 1.IOLoop.add_callback(callback, *args, **kwargs)
这个函数是最简单的，在ioloop开启后执行的回调函数callback，*args和**kwargs都是这个回调函数的参数。一般我们的server都是单进程单线程的，即使是多线程，那么这个函数也是安全的。

##### 2.IOLoop.add_callback_from_signal(callback, *args, **kwargs)
这个函数和上面的很类似，只不过他是在without any stack_context的时候去执行，关于stack_context，查阅[这里](http://www.tornadoweb.org/en/stable/stack_context.html#module-tornado.stack_context)。

##### 3.IOLoop.add_future(future, callback)
这个函数呢也是添加一个callback函数，当给定的这个future执行完的时候，callback会去执行，这个函数有唯一的一个参数就是这个future对象。关于future呢，后面会详细去讲。