###2.自动收集rpc函数

#####服务器可被客户端调用的函数，不会有多少就去写多少到字典中, 所以需要自动去收集可被调用函数,这样就方便许多。
```
collect.py

#1.第一种方法也比较傻，适合rpc文件较少的情况
import rpc1

DICT = {}

for name in dir(rpc1):
    if name.startswith("__") or name.endswith("__"):
        continue
    DICT[name] = getattr(rpc1, name)

rpc1.py

#被调用函数
def func():
    return 1

```
```
#第二种相对智能一些
将所有可被调用函数的文件写入固定文件夹中，比如取名为handler的文件夹。直接将模块import或者也可以写入一个字典中
#name为当前文件与handler的相对路径
_imported = []
for f in os.listdir(name + "/handler"):
    if f.find('.pyc') > 0:
        _subfix = '.pyc'
    elif f.find('.pyo') > 0:
        _subfix = '.pyo'
    elif f.find('.py') > 0:
        _subfix = '.py'
    else:
        continue

    fname, _ = f.rsplit(_subfix, 1)
    if fname and fname not in _imported:
        _handlers_name = '%s.%s' % (module, fname)
        __import__(_handlers_name)
        _imported.append(fname)

```

