## RPC on my client

##### 服务器调用client函数也是一样的道理，client中也要和服务器一样，将想要被调用的函数收集起来。当然这个时候，服务器还要做一件事情，就是将客户端的tcp连接通过某种条件储存起来，这样才知道我想去调用哪个client的函数。

##### 现在有很多client来连接我们server，以游戏为例，每个玩家都有自己对应的uid，那么我们就可以将conn根据uid储存起来。```client_conn = {uid:conn, ..}``` 这样，我们想给哪位玩家发送消息或者做一些其他的事情，就很容易了，只要客户端写好函数功能，我们只要将函数名字，以及对应参数，通过client_conn找到对应玩家的tcp连接，然后将上述数据发送过，那么当客户端计算出结果以后，我们通过一个回调函数，就可以将结果return到服务器了。

现有的rpc框架很多，比如[msgpack-python](https://github.com/msgpack-rpc/msgpack-rpc-python), 这里有很多语言的rpcSimpleServer，感兴趣的同学可以参考一下。