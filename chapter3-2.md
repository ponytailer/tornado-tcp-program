## 3.2 baseIOStream的相关函数以及IOStream类

###### 1.BaseIOStream.fileno()
这个很简单，就是我们stream当前的文件描述符。

##### 2.BaseIOStream.close_fd()
关闭这个fd。

##### 3.BaseIOStream.write_to_fd(data)
尝试向这个fd去写data， 期间可能会出现未成功写入，因此函数的返回值是成功写入数据的大小。

##### 4.BaseIOStream.read_from_fd(data)
有写也有读。

##### 5.BaseIOStream.get_fd_error()
获取fd中所有error信息。


关于baseIOStream的信息差不都就这些了，在我们实际使用的过程中，我们不会去直接使用这个类的，我们基本都会去使用它的子类，IOStream。

以下通过官方文档的例子，来简单介绍一下IOStream。
```
import tornado.ioloop
import tornado.iostream
import socket

def send_request():
    stream.write(b"GET / HTTP/1.0\r\nHost: friendfeed.com\r\n\r\n")
    stream.read_until(b"\r\n\r\n", on_headers)

def on_headers(data):
    headers = {}
    for line in data.split(b"\r\n"):
       parts = line.split(b":")
       if len(parts) == 2:
           headers[parts[0].strip()] = parts[1].strip()
    stream.read_bytes(int(headers[b"Content-Length"]), on_body)

def on_body(data):
    print(data)
    stream.close()
    tornado.ioloop.IOLoop.current().stop()

if __name__ == '__main__':
    #创建一个socket
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM, 0)
    #创建一个stream
    stream = tornado.iostream.IOStream(s)
    #连接目标，执行回调函数send_request
    stream.connect(("friendfeed.com", 80), send_request)
    开启ioloop
    tornado.ioloop.IOLoop.current().start()
```