##3.3 Tcp Client和Tcp Server

####  tornado.tcpclient.TCPClient(resolver=None, io_loop=None)
这个呢就是tornado自有的一个tcpclient，使用它的时候，可以直接继承它.

它有一个方法
```connect(host, port, af=<AddressFamily.AF_UNSPEC: 0>, ssl_options=None, max_buffer_size=None)```

它会返回IOStream的实例，如果ssl_options为真，那么会返回SSLIOStream。

通过给的host和port来连接服务器，然后通过返回的stream，就可以进行读写等操作了。


#### tornado.tcpserver.TCPServer(io_loop=None, ssl_options=None, max_buffer_size=None, read_chunk_size=None)

这个就是用来创建TCPserver的。它是非阻塞，单线程的。

来看一下关于它的几个函数

##### 1.listen(port, address="")
server = TCPServer()
server.listen(8888)
IOLoop.current().start()