##TcpServer类的解读

#####在TCPServer类的注释中，首先强调了它是一个non-blocking, single-threaded TCP Server.

那么如何理解non-blocking呢？
non-blocking，就是说，这个服务器没有使用阻塞式API。
通常来说，我们socket的读写都是阻塞式的,不管有没有数据，服务器都派API去读，读不到，API就不会回来交差。而非阻塞区别在于没有数据可读时，它不会在那死等，它直接就返回了。
而single-thread，说的是服务器是单线程模式，一个线程可以监视成千上万的连接，因此不需要多线程。在ubuntu上用的是epoll，bsd用的是kqueue。

在使用方面，tcpserver这个类一般不直接使用，而是派生出子类，然后让子类实例化。可以看到作者是强制去继承tcpserver，handle_stream并没有实现，如果你不去覆盖，就会报错。
```
def handle_stream(self, stream, address):

    """Override to handle a new `.IOStream` from an incoming connection."""

    raise NotImplementedError()```


