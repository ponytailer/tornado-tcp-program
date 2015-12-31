##tornado 在TCP里的工作

首先是关于 TCP 协议。这是一个面向连接的可靠交付的协议。由于是面向连接，所以在服务器端需要分配内存来记忆客户端连接，同样客户端也需要记录服务器。为了安全所以有了三次握手机制，这里给出一张图-- 状态转换图(UNIX网络编程)![](2013_12_06_01.png)


对于 TCP 编程的总结就是：创建一个监听 socket，然后把它绑定到端口和地址上并开始监听，然后不停 accept。这也是 tornado 的 TCPServer 要做的工作。

TCPServer 类的定义在 tcpserver.py。它有两种用法：bind+start 或者 listen。