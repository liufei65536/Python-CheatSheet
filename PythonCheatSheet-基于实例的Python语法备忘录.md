# 前言
今天想写一个上下文管理器，发现自己虽然知道这个概念，也知道要用`contextmanager`来写，但是要动手写时却发现不知道怎么下手。于是想起做一个备忘录，记录一些有用但可能不那么常用的语法。平时也会遇到很多操作，写法比较固定，记录下来方便重用，例如用matplotlib画图，连接mysql数据集，PyTorch写一个train loop,......

本着不重复造轮子的思想，如果该操作官网已有，就直接给出官网链接。

# Python常用操作
## 上下文管理器
来自：[contextlib.contextmanager](https://docs.python.org/zh-cn/3/library/contextlib.html#contextlib.contextmanager)
### 例
下面实现了一个抽象的资源管理器。
用于请求资源和释放资源。
```py
from contextlib import contextmanager
@contextmanager
def managed_resource(*args, **kwds):
    # Code to acquire resource, e.g.:
    resource = acquire_resource(*args, **kwds)
    try:
        yield resource
    finally:
        # Code to release resource, e.g.:
        release_resource(resource)
```
```py
#Usage
>>> with managed_resource(timeout=3600) as resource:
...     # Resource is released at the end of this block,
...     # even if code in the block raises an exception
```

### 说明
`@contextlib.contextmanager`函数是一个`decorator`，它可以定义一个支持`with`语句上下文管理器的工厂函数， 而不需要创建一个类或单独的 `__enter__()` 与 `__exit__()` 方法。



上面是一个抽象的示例，展示如何确保正确的资源管理。

被装饰的函数在被调用时，必须返回一个 `generator` 迭代器。 这个迭代器必须只 `yield` 一个值出来，这个值会被用在 `with` 语句中，绑定到 `as` 后面的变量（如果有的话）。

当生成器发生`yield` 时， `with` 中的语句体会被执行。 语句体执行完毕离开之后，该生成器将被恢复执行。 如果在该语句体中发生了未处理的异常，则该异常会在生成器发生`yield` 时重新被引发。 因此，你可以使用 `try...except...finally` 语句来捕获该异常（如果有的话），或确保进行了一些清理。 如果仅出于记录日志或执行某些操作（而非完全抑制异常）的目的捕获了异常，生成器必须重新引发该异常。 否则生成器的上下文管理器将向 with 语句指示该异常已经被处理，程序将立即在 with 语句之后恢复并继续执行。

`contextmanager()` 使用 `ContextDecorator` 因此它创建的上下文管理器不仅可以用在 `with` 语句中，还可以用作一个装饰器。当它用作装饰器时，每次函数调用时都会隐式创建一个新的生成器实例（这使得 `contextmanager()` 创建的上下文管理器满足了支持多次调用以用作装饰器的需求，而非“一次性”的上下文管理器）。
[contextlib.ContextDecorator](https://docs.python.org/zh-cn/3/library/contextlib.html#contextlib.ContextDecorator)

## 命令行参数
[教程how-to argparse](https://docs.python.org/zh-cn/3/howto/argparse.html#id1)

```py
#test_args.py
import argparse


parser = argparse.ArgumentParser(description='Process some integers.')
parser.add_argument("--log", default='INFO', help="log level")
parser.add_argument("--logfile", default='example.log', help="log file path")
args = parser.parse_args()
loglevel = args.log
logfile = args.logfile
```
使用：
```py
python test_args.py --log=DEBUG --logfile=test.log
```

## 日志logging
参考文档：
[basic](https://docs.python.org/zh-cn/3/howto/logging.html#logging-basic-tutorial)
[advanced](https://docs.python.org/zh-cn/3/howto/logging.html#logging-advanced-tutorial)
[cookbook](https://docs.python.org/zh-cn/3/howto/logging-cookbook.html#logging-cookbook)
[崔庆才 logging 模块的基本用法](https://cuiqingcai.com/6080.html)
在日志文件记录程序运行过程。
可以在命令行用`--log`参数调整日志记录级别，用`--logfile`参数设置日志存储路径：
例如：
`python learn-logging.py --log=DEBUG --logfile=test.log`

```py
import logging
import argparse


parser = argparse.ArgumentParser(description='Process some integers.')
parser.add_argument("--log", default='INFO', help="log level")
parser.add_argument("--logfile", default='example.log', help="log file path")
args = parser.parse_args()
loglevel = args.log
logfile = args.logfile

numeric_level = getattr(logging, loglevel.upper(), None)
if not isinstance(numeric_level, int):
    raise ValueError('Invalid log level: %s' % loglevel)

logging.basicConfig(level=numeric_level, filename=logfile,
                    format='%(asctime)s:%(levelname)s: %(message)s')

logging.debug('debug...')
logging.info('info...')
logging.warning('warning...')
logging.error('error...')
```

注意：`logging.basicConfig`是一次性的，只有第一次设置有效。如果你导入的模块已经设置过了，会导致你的设置无效。
