## 2.3 ioloop基础函数相关说明

1.add_handler, update_handler, remove_handler三个和handelr有关的函数，通常是在写connection的时候用到的，刚开始学习的时候，不用太在意它的用法，在后面会详细说明。

2.add_callback是比较常用的函数之一，不管在tornado还是其他的异步框架中，回调都是非常常见的。