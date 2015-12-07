## RPC on my server


##### 下面是之前我们给出的tcpserver中的一个函数，用来处理连接
```    
def _handle_connect(self, sock):
    conn = self._build_class(sock, **self._build_kwargs)
    self.on_connect(conn)

    close_callback = functools.partial(self.on_close, conn)
    conn.set_close_callback(close_callback)
    
```

##### build_class之前我们说过，这是我们用来处理数据的protocol，那么rpc的逻辑流程应该都写在这里。现在假设client来调用一个函数sum(x, y)， 那么在我们server中就要有这样一个函数。

```
def sum(x, y):
    return x+y```
    
但是客户端来调用，我们如何知道server有这样一个函数呢？这就需要我们提前去处理一下，将希望可以被client调用的函数存起来。

def route(**options):
    def decorator(handler):
        msgid = options.pop('msgid', handler.__name__)
        elif not msgid in HANDLERS:
            HANDLERS[msgid] = handler
        else:
            raise Exception('[ ERROR ]Handler "%s" exists already!!' % msgid)
        return handler
    return decorator