#2.ioloop
说到tornado，那就不得不说他的ioloop，这是这个框架的灵魂所在。通过一段简单的代码，来开启tornado tcp编程的大门。

```python
import errno
import functools
import tornado.ioloop
import socket

def connection_ready(sock, fd, events):
    while True:
        try:
            connection, address = sock.accept()
        except socket.error as e:
            if e.args[0] not in (errno.EWOULDBLOCK, errno.EAGAIN):
                raise
            return
        connection.setblocking(0)
        handle_connection(connection, address)

if __name__ == '__main__':
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM, 0)
    sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    sock.setblocking(0)
    sock.bind(("", port))
    sock.listen(128)

    io_loop = tornado.ioloop.IOLoop.current()
    callback = functools.partial(connection_ready, sock)
    io_loop.add_handler(sock.fileno(), callback, io_loop.READ)
    io_loop.start()
```

这是官方文档给出的一段简单的tcpserver的代码，那么我们就从它来分析一下。
首先是socket的部分，创建了一个socket，然后下面是ioloop的部分。首先我们创建一个ioloop实例, 然后创建了一个回调函数, （partial这个函数不用太关心，就是一种绑定参数的函数写法。）然后给ioloop加上一个handler，用于监听socket，最后开启ioloop。当ioloop开启以后，就会去执行回调函数connecction_ready。

以上就是创建一个simpleTCPServer的步骤，接下来我们主要围绕ioloop来说一下与它相关的函数方法。