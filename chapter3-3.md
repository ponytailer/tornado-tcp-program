##3.3 Tcp Client和Tcp Server

###### 1。tornado.tcpclient.TCPClient(resolver=None, io_loop=None)
这个呢就是tornado自有的一个tcpclient，使用它的时候，可以直接继承它.

它有一个方法
```connect(host, port, af=<AddressFamily.AF_UNSPEC: 0>, ssl_options=None, max_buffer_size=None)```