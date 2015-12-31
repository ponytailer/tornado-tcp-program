##tornado 在TCP里的工作

首先是关于 TCP 协议。这是一个面向连接的可靠交付的协议。由于是面向连接，所以在服务器端需要分配内存来记忆客户端连接，同样客户端也需要记录服务器。为了安全所以有了三次握手机制，这里给出一张图-- 状态转换图(UNIX网络编程)![](2013_12_06_01.png)


对于 TCP 编程的总结就是：创建一个监听 socket，然后把它绑定到端口和地址上并开始监听，然后不停 accept。这也是 tornado 的 TCPServer 要做的工作。

TCPServer 类的定义在 tcpserver.py。它有两种用法：bind+start 或者 listen。

简言之，基于事件驱动的服务器（tornado）要干的事就是：创建 socket，绑定到端口并 listen，然后注册事件和对应的回调，在回调里accept 新请求。


创建监听 socket 后为了异步，设置 socket 为非阻塞（这样由它 accept 派生的socket 也是非阻塞的），然后绑定并监听之。add_sockets 方法接收 socket 列表，对于列表中的 socket，用 fd 作键记录下来，并调用add_accept_handler 方法。它也是在 netutil 里定义的。