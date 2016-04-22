###3.和数据库的那些事

#####在开发中,数据库是必不可少的, 因此这节主要来说一下常用的两种类型数据库,mysql和redis的简单使用
```


#1.mysql算是最常用的数据库之一了,不要钱,功能齐全,性能优良。这里主要使用tornado的一款api,
tornado_mysql。

from tornado_mysql      import pools
from tornado            import gen
from tornado            import ioloop
from tornado.concurrent import Future
from pool               import threadpool

import functools

SYNC, ROW, DATASET = range(3)

__pool = None

ioloop = ioloop.IOLoop.current()

def init(**conf):
    global __pool

    if not __pool:
        __pool = pools.Pool(conf, max_idle_connections=5, max_recycle_sec=3)

@gen.coroutine
def execute(sql, value=None, operator=SYNC):
    assert __pool is not None
    
    result = None
    if value is None:
        cur = yield __pool.execute(sql)
    else:
        yield __pool.execute(sql, value)
    if operator == ROW:
        result = cur.fetchone()
    elif operator == DATASET:
        result = cur.fetchall()
    raise gen.Return(result)

fetchone = functools.partial(execute, operator=ROW)
fetchall = functools.partial(execute, operator=DATASET)
对几种简单的数据库操作进行了简单的封装,便于开发中的使用。
```



```
#2.redis算是比较常用的数据库之一，一般使用来做cache,也有做消息队列的。这里用的是
tornadoredis这款api。

import tornadoredis
from tornado import gen
from tornado.concurrent import Future

pool = None

def init():
    global pool

    if not pool:
        CONNECTION_POOL = tornadoredis.ConnectionPool(max_connections=500, wait_for_available=True)
        pool = tornadoredis.Client(connection_pool=CONNECTION_POOL, selected_db=12) 

@gen.coroutine
def close():
    if pool:
        yield pool.disconnect()

@gen.coroutine
def batch(commands):
    assert pool is not None
    result = None
    try:
        pipe = pool.pipeline()

        for _cmd in commands:
            _op   = _cmd[0]
            _args = _cmd[1]

            if len(_cmd) > 2:
                _kwargs = _cmd[2]
            else:
                _kwargs = {}

            getattr(pipe, _op)(*args, **kwargs)
            
        result = yield gen.Task(pipe.execute)
    except Exception as e:
        print e

    raise Return(result)

def execute(cmd, *args, **kwargs):
    assert pool is not None
    f = Future()
    def onResult(result):
        f.set_result(result)

    result = None
    _func  = getattr(pool, cmd)
    _func(callback=onResult, *args, **kwargs)
    return f

#对redis的操作进行了统一封装,直接使用execute就可以, 将命令作为参数传递。多个命令的执行可以使用batch来执行, 使用pipe提高执行效率。
```

