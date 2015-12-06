## 3.1BaseIOStream

简单的来说，这个类从socket中读或写数据。


以下是它的几个基本属性：
```
io_loop – 当前的ioloop实例。
max_buffer_size – Maximum amount of incoming data to buffer; defaults to 100MB.
read_chunk_size – Amount of data to read at one time from the underlying transport; defaults to 64KB.
max_write_buffer_size – Amount of outgoing data to buffer; defaults to unlimited.
```