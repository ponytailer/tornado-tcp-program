## 2.3 ioloop相关函数说明

1.add_handler, update_handler, remove_handler三个和handelr有关的函数，通常是在写connection的时候用到的，刚开始学习的时候，不用太在意它的用法，在后面会详细说明。

2.add_callback是比较常用的函数之一，不管在tornado还是其他的异步框架中，回调都是非常常见的。在服务器启动以后，会有一些相关的操作需要在ioloop启动后才能有效的去执行，那么这个时候，添加一个callback就是非常必要的了。以游戏服务器为例子(可能不贴切)，在游戏服务器启动后，会把全服的排行榜数据load到内存中，那么这个时候就可以使用add_callback了。
```
io_loop = ioloop.IOLoop.instance()
io_loop.add_callback(load_rank_list)
io_loop.start()
```
其中load_rank_list就是去将排行榜信息load到内存中的相关操作。


3.call_later和call_at也是我们在开发过程中常用的函数之一。它们给我们在做相关定时的操作的时候带来了便利。举个简单的例子，在晚上9点需要向玩家发放一些奖励道具，那么我就可以用到call_at

``` call_at(time, func, *args, **kw)```

time就是晚上9点的时间戳，func就是发放奖励的函数，后面是相关参数。call_later同理，在第一次发完以后，我们就可以使用call_later()来进行一个循环的操作，
```call_later(86400, func)```
这样每过86400s(一天)，就可以再次发放奖励。


4.ioloop的完全开启，也就相当于我们整个服务器开启，如果想要获得服务器从开启到现在的过了多久，那么IOLoop.time()就可以帮助我们得到它。