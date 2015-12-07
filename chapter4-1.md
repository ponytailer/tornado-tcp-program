## RPC on my server


下面是之前我们给出的tcpserver中的一个函数，用来处理连接
```    
def _handle_connect(self, sock):
    conn = self._build_class(sock, **self._build_kwargs)
    self.on_connect(conn)

    close_callback = functools.partial(self.on_close, conn)
    conn.set_close_callback(close_callback)
```