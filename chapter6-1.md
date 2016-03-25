###1.正确关闭服务器的姿势

#####有时候，服务器的进程因为某种原因被关闭或者自己手动关闭，无法保证内存里的数据正确入库或者仍有cb函数没有执行，这个时候，我们就要确保一切都正确的被执行完毕后，再关闭服务器。
```
def sig_handler(sig, frame):
    logger.warning('Caught signal: %s', sig)
    ioloop.IOLoop.instance().add_callback(shutdown)

def shutdown():
    io_loop = ioloop.IOLoop.instance()
    server.stopFactory() ##自己去做一些处理，保证入库等。

    deadline = time.time() + 5

    def stop_loop():
        now = time.time()
        if now < deadline and io_loop._callbacks:
            io_loop.add_timeout(now + 1, stop_loop)
        else:
            io_loop.stop() # 处理完现有的 callback后，结束ioloop循环
    stop_loop()

def start():
    log_initialize()
    global server
    server = RPCServer(('localhost', 5700))
    server.start()

    signal.signal(signal.SIGTERM, sig_handler)
    signal.signal(signal.SIGINT, sig_handler)
    ```