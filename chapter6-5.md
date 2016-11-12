###tornado和celery很配呦


Celery 是一个简单、灵活且可靠的，处理大量消息的分布式系统，并且提供维护这样一个系统的必需工具。它是一个专注于实时处理的任务队列，同时也支持任务调度。在我们日常的开发中，或多或少都会用到，一些比较耗时的异步任务，一些定时的任务都可以用celery去做。

在tornado中如果想使用celery，首先要安装celery的python api。使用pip install celery就可以了，非常方便。

```from celery import Celery, task

c = Celery()

这是最基本的用法。

我们想要定义一个任务，我们就可以写一个很普通的方法，比如

@task
def mytask(a):
    print 1111
    
只要加上@task这样一个装饰器，它就会标识为一个celery的task。我们就可以调用他了。

首先我们要启动celery，下面是我的启动命令
celery -A my.utils.async_task worker -P gevent -c 2 -l info -n 'my.worker.%%h.%(ENV_USER)s'

相关参数官方文档中都可以查到，这里就不一一详述了。

调用的时候，我们要这样

v = mytask.apply_async(111, countdown=1)
他的返回值是他的任务id，通过任务id，我们可以取消任务,countdown代表多少秒以后执行

revoke函数就是取消任务的，revoke(task_id)```