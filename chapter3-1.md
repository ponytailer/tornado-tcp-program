## 3.1BaseIOStream

简单的来说，这个类从socket中读或写数据。


以下是它的几个基本属性：
```
io_loop – 当前的ioloop实例。
max_buffer_size – 最大的可接受数据大小，默认是100M。
read_chunk_size – 读取的数据大小，默认64k。
max_write_buffer_size – 最大的写buffer大小。  
```
这些都可以在继承的时候修改的。

介绍一下主要的几个接口

##### 1.BaseIOStream.write(data, callback=None)
异步的写数据，如果有callback，那么在所有数据成功写入以后执行callback。


##### 2.BaseIOStream.read_bytes(num_bytes, callback=None, streaming_callback=None, partial=False)

异步的读取数据，读到的数据大小取决于num_bytes。同理，如果有callback，在数据完全读取后，执行callback。读取到的数据data作为callback的参数，如果不是的话，那么callback需要返回一个future对象。而streaming_callback,是当读取到的数据全都是有效的情况下，才会去执行。

##### 3.BaseIOStream.read_until(delimiter, callback=None, max_bytes=None)

同上，只不过是当读delimiter(分隔符)的时候停止。callback和read_bytes里的一样。

##### 4.BaseIOStream.read_until_regex(regex, callback=None, max_bytes=None)
通过正则来读取数据，regex就是给定的正则表达式。

##### 5.BaseIOStream.read_until_close(callback=None, streaming_callback=None)
异步读数据，知道socket关闭。

##### 6.BaseIOStream.close(exc_info=False)
关闭当前的stream。

##### 7.BaseIOStream.set_close_callback(callback)
当stream关闭时，执行的回调函数。