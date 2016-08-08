
###跟多线程搞一些事情

至今我仍然坚信，单线程仍然是最有效率的方式，比如nginx就是个很好的例子，在python中尤其。但是tornado依然提供了多线程方式来给开发者。这里给出一个例子，并做出简单的解释。
```
EXECUTOR = ThreadPoolExecutor(max_workers=4)＃最大线程数

def unblock(f):
    @tornado.web.asynchronous
    @wraps(f)
    def wrapper(*args, **kwargs):
        self = args[0]
        def callback(future):
            self.write(future.result())#将f结果写回给client
            self.finish()

        EXECUTOR.submit(
                partial(f, *args, **kwargs)#启用多线程
                ).add_done_callback(#因为是异步执行，所以返回值是future
                        lambda future: tornado.ioloop.IOLoop.instance().add_callback(
                            partial(callback, future)))#最后写回client的步骤要在主线程中完成，否则会出错，因此需要通过回调来将f返回的future返回到主线程中。
    return wrapper

class MainHandler(tornado.web.RequestHandler):
    @unblock
    def get(self):
        sleep(3)
        #self.write("ff")
        return "ff"
        
```
线程库用的是futures里提供的，比较简单，没有安装的童鞋，可以用pip install futures 来进行安装.
上面的例子用的是最简单的http请求，tcp中也是同样一个道理，只需要讲self.write和finish替换成自己写socket的函数就可以了。
