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