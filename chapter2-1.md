
## 2.1  ioloop开启时的基本函数




##### 1.IOLoop.current(instance=True)

如果当前的ioloop已经运行了，那么这个函数就是用来获得当前线程里的IOLoop对象。

##### 2.IOLoop.start()
这个函数就很简单了，就是开启我们的ioloop，它会一直运行下去，直到有人调用了stop()。

##### 3.IOLoop.stop()
这个就是上面说的用来停止ioloop循环的。

##### 4.IOLoop.run_sync(func, timeout=None)
这个函数是在ioloop开启时去执行func这个函数，然后关闭ioloop，func执行完它会自动执行stop()，这个不用担心。
```
@gen.coroutine
def main():
    # do sth...

if __name__ == '__main__':
    IOLoop.current().run_sync(main)
    
```

##### 5.add_handler(fd, handler, events)
注册一个handler，从fd那里接受事件。
fd呢就是一个描述符，events就是要监听的事件。
events有这样几种类型，IOLoop.READ, IOLoop.WRITE, 还有IOLoop.ERROR.
很好理解，读写事件，还有错误异常。

当我们选定的类型事件发生的时候，那么就会执行handler(fd, events)。


##### 6.update_handler(fd, events)
用来更新上面我们注册的handler的。

##### 7.remove_handler(fd)
停止监听fd上面的所有事件。



以上就是在ioloop开启的时候，涉及到的主要函数及其作用的介绍。
