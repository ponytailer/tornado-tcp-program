##3.4 一个简单的TCPServer

```
class TcpServer(object):
    def __init__(self, address, build_class, **build_kwargs):
        self._address = address
        self._build_class = build_class
        self._build_kwargs = build_kwargs

    def _accept_handler(self, sock, fd, events):
        while True:
            try:
                获得conn
                connection, address = sock.accept()
            except socket.error, e:
                if errno_from_exception(e) not in _ERRNO_WOULDBLOCK:
                    raise
                return
            
            #通过conn解析
            self._handle_connect(connection)

    def _handle_connect(self, sock):
        #这里的conn主要是我们来解析数据的protocol
        conn = self._build_class(sock, **self._build_kwargs)
        self.on_connect(conn)

        close_callback = functools.partial(self.on_close, conn)
        #设置一个conn关闭时执行的回调函数
        conn.set_close_callback(close_callback)

    def startFactory(self):
        pass

    def start(self, backlog=0):
        #创建socket
        socks = build_listener(self._address, backlog=backlog)

        io_loop = ioloop.IOLoop.instance()
        for sock in socks:
            #接受数据的handler
            callback = functools.partial(self._accept_handler, sock)
            #为ioloop添加handler，callback
            io_loop.add_handler(sock.fileno(), callback, WRITE_EVENT | READ_EVENT | ERROR_EVENT)
        #在ioloop开启后，添加一个回调函数
        ioloop.IOLoop.current().add_callback(self.startFactory)
    
    #接受buff的函数，继承tcpserver的时候可以重写。
    def handle_stream(self, conn, buff):
        logger.debug('handle_stream')

    def stopFactory(self):
        pass

    def on_close(self, conn):
        logger.debug('on_close')

    def on_connect(self, conn):

        logger.debug('on_connect: %s' % repr(conn.getaddress()))

        handle_receive = functools.partial(self.handle_stream, conn)
        conn.read_util_close(handle_receive)
    ```
    
我们来看一下流程，
#####1.start().我们开启tcpserver，首先创建一个socket，, 然后我们为ioloop添加handler。
    
#####2.服务器开启后，执行_accept_handler,这个函数里是一个循环，从socket中获取conn
#####3.然后我们解析这个conn， build_class使我们自己实现的一个用来解析数据，拆包解包的一个protocol类，这里我们会将所有的bytes解析为我们能看懂的字符串数字等等。
#####4.然后我们调用on_connect函数，我们将这个conn传入handler_stream中， 前面讲过，这个函数是用来接受的stream的。然后我们从stream中读取数据，知道conn关闭。