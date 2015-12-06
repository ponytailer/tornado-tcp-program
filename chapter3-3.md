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
```
server = TCPServer()
server.listen(8888)
IOLoop.current().start()
```

这就可以创建一个简单的tcpserver，端口号8888

##### 2.bind(port, address=None, family=<AddressFamily.AF_UNSPEC: 0>, backlog=128)
开启多进程的一个方法
```
server = TCPServer()
server.bind(8888)
server.start(0)  # Forks multiple sub-processes
IOLoop.current().start()
```


##### 3.add_sockets(sockets)
也可以开启多进程。
```
sockets = bind_sockets(8888)
tornado.process.fork_processes(0)
server = TCPServer()
server.add_sockets(sockets)
IOLoop.current().start()
```

##### 4.handle_stream(stream, address)
这是我们用来接收stream的方法。你可以通过继承TCPServer，来覆盖这个方法。

