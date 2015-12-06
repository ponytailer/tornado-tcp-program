##3.3 Tcp Client和Tcp Server

###### 1。tornado.tcpclient.TCPClient(resolver=None, io_loop=None)
这个呢就是tornado自有的一个tcpclient，使用它的时候，可以直接继承它.

它有一个方法
```connect(host, port, af=<AddressFamily.AF_UNSPEC: 0>, ssl_options=None, max_buffer_size=None)```

它会返回IOStream的实例，如果ssl_options为真，那么会返回SSLIOStream。

通过给的host和port来连接服务器，然后通过返回的stream，就可以进行读写等操作了。


2.tornado.tcpserver.TCPServer(io_loop=None, ssl_options=None, max_buffer_size=None, read_chunk_size=None)