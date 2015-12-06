## 3.1BaseIOStream

简单的来说，这个类从socket中读或写数据。


以下是它的几个基本属性：
```
io_loop – 当前的ioloop实例。
max_buffer_size – 最大的可接受数据大小，默认是100M。
read_chunk_size – 读取的数据大小，默认64k。
max_write_buffer_size – Amount of outgoing data to buffer; defaults to unlimited.
```