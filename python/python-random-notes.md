# python random notes

```python
sys.version # check the python version
locals() # show current local symbol table
globals() # global symbol table

#python 3
import urllib.parse
# can't import urllib and then try to access urllib.parse
urllib.parse.quote_plus(s)

#python2
urllib.quote_plus(s)
```

DictWriter and imap

```python
from csv import DictWriter
from multiprocessing import Pool
import time

def process_line(line):
  # biz logic
  return {
    'name': name,
    'age': age,
  }

NUM_OF_WORKERS = 8

pool = Pool(NUM_OF_WORKERS)
with open('input.csv', 'r') as input, \
     open('output.csv', 'w', newline ='') as output:
  headers = [
    'name',
    'age',
  ]
  writer = DictWriter(output, fieldnames = headers)
  writer = writeheader()

  s = time.time()
  # imap returns one result as the worker finishes one task
  # map returns all results when all tasks are done
  for result in pool.imap(process_line, input, NUM_OF_WORKERS):
    writer.writerow(result)
  e = time.time()
  rint(f'finished in {e-s} seconds.')
```

More on map vs imap

{% embed url="https://stackoverflow.com/questions/26520781/multiprocessing-pool-whats-the-difference-between-map-async-and-imap" %}

## pyenv

{% embed url="https://blog.laisky.com/p/pyenv/" %}

建议在本地和服务器上都通过 pyenv 来管理版本，而不要去动系统自带的 python（以免引起额外的麻烦） [https://blog.laisky.com/p/pyenv/](https://t.co/FgPEmYvVyE?amp=1) 另外一点就是，如果你想写一个兼容 2、3 的工具包，你可以考虑使用 future [http://python-future.org/compatible\_idioms.html…](https://t.co/eEcCbvwi76?amp=1) 最后提醒一下，2to3 这个脚本是有可能出错的。

## Python Style Guide

PEP 8

{% embed url="https://python.freelycode.com/contribution/detail/47" %}

Google python guide

{% embed url="https://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/python\_language\_rules/\#" %}

注意事项

{% embed url="https://laisky.github.io/style-guide-cn/style-guides/consensuses/" %}

## python 3 built in module

{% embed url="https://pymotw.com/3/" %}

## Other random python guide

{% embed url="https://blog.laisky.com/p/python-road/" %}

## 并行

以前听别人讲过一个比喻，静态语言是吃冒菜，一次性烫好。而动态语言是涮火锅，吃一点涮一点。

那么我觉得，GIL 就是仅有一双筷子的火锅，即使你菜很多，一次也只能涮一个。

但是，对于 I/O bound 的操作，你不必一直夹着菜，而是可以夹一些扔到锅里，这样就可以同时涮很多，提高并行效率。

并行时主要是在考虑两个问题：同步控制和资源用量。

### 同步控制

thread, multiprocessing, asyncio 几个包里都会发现一系列的工具：

* Lock 互斥锁
* RLock 可重入锁
* Queue 队列
* Condition 条件锁
* Event 事件锁
* Semaphore 信号量

这个就不展开细谈了，属于另一个语言无关的大领域。

{% embed url="https://blog.laisky.com/p/concurrency-lock/" %}

{% embed url="https://blog.laisky.com/p/os-process/\#%E6%95%8F%E6%84%9F%E5%8C%BA%E9%97%AE%E9%A2%98" %}

资源用量

* 缓冲区buffer有多大（Queue 长度）
* 并发量有多大（workers 数量）

一般来说，前者直接确定了你内存的消耗量，最好选择一个恰好或略高于消费量的数。后者一般直接决定了你的 CPU 使用率，过高的并发量会增加切换开销，得不偿失。

#### 池pool

我们经常提到线程池、进程池、连接池。说白了就是对于一些可重用的资源，不必每次都创建新的，而是使用完毕后回收留待下一个数据继续使用。比如你可以选择不断地开子线程，也可以选择预先开好一批线程，然后通过 queue 来不断的获取和处理数据。

所以说使用“池”的主要目的就是减少资源的消耗。另一个优点是，使用池可以非常方便的控制并发度（很多新人以为 Queue 是用来控制并发度的，这是错误的，Queue 控制的是缓冲量）。 对于连接池，还有另一层好处，那就是端口资源是有限的，而且回收端口的速度很慢，你不断的创建连接会导致端口迅速耗尽。

对前面介绍的 python 中进程／线程做一个小结，线程池可以用来解决 I/O 的阻塞，而进程可以用来解决 GIL 对 CPU 的限制（因为每一个进程内都有一个 GIL）。所以你可以开 N 个（小于等于核数）进程池，然后在每一个进程中启动一个线程池，所有的线程池都可以订阅同一个 Queue，来实现真正的多核并行。

非常简单的描述一下进程／线程，对于操作系统而言，可以认为进程是资源的最小单位（在 PCB 内保存如图 1 的数据）。而线程是调度的最小单位。同一个进程内的线程共享除栈和寄存器外的所有数据。 所以在开发时候，要小心进程内多线程数据的冲突，也要注意多进程数据间的隔离（需要特别使用进程间通信）

不过在 Py 里，单机上最常用的进程间通信就是 multiprocessing 里的 Queue 和 sharedctypes。

做一个小结，一个简单的做法是，启动程序后，分别创建一个进程池（进程数小于等于可用核数）、线程池和 ioloop，ioloop 负责调度一切的协程，遇到阻塞的调用时，I/O 型的扔进线程池，CPU 型的扔进进程池，这样代码逻辑简单，还能尽可能的利用机器性能。

例子：

```python
"""
✗ python process_thread_coroutine.py

[2019-08-11 09:09:37,670Z - INFO - kipp] - main running...
[2019-08-11 09:09:37,671Z - INFO - kipp] - coroutine_main running...
[2019-08-11 09:09:37,671Z - INFO - kipp] - io_blocking_task running...
[2019-08-11 09:09:37,690Z - INFO - kipp] - coroutine_task running...
[2019-08-11 09:09:37,691Z - INFO - kipp] - coroutine_error running...
[2019-08-11 09:09:37,691Z - INFO - kipp] - coroutine_error end, cost 0.00s
[2019-08-11 09:09:37,693Z - INFO - kipp] - cpu_blocking_task running...
[2019-08-11 09:09:38,674Z - INFO - kipp] - io_blocking_task end, cost 1.00s
[2019-08-11 09:09:38,695Z - INFO - kipp] - coroutine_task end, cost 1.00s
[2019-08-11 09:09:39,580Z - INFO - kipp] - cpu_blocking_task end, cost 1.89s
[2019-08-11 09:09:39,582Z - INFO - kipp] - coroutine_main got [None, AttributeError('yo'), None, None]
[2019-08-11 09:09:39,582Z - INFO - kipp] - coroutine_main end, cost 1.91s
[2019-08-11 09:09:39,582Z - INFO - kipp] - main end, cost 1.91s
"""


from time import sleep, time
from asyncio import get_event_loop, sleep as asleep, gather, ensure_future, iscoroutine
from concurrent.futures import ProcessPoolExecutor, ThreadPoolExecutor, wait
from functools import wraps

from kipp.utils import get_logger


logger = get_logger()


N_FORK = 4
N_THREADS = 10

thread_executor = ThreadPoolExecutor(max_workers=N_THREADS)
process_executor = ProcessPoolExecutor(max_workers=N_FORK)
ioloop = get_event_loop()


def timer(func):
    @wraps(func)
    def wrapper(*args, **kw):
        logger.info(f"{func.__name__} running...")
        start_at = time()
        try:
            r = func(*args, **kw)
        finally:
            logger.info(f"{func.__name__} end, cost {time() - start_at:.2f}s")

    return wrapper


def async_timer(func):
    @wraps(func)
    async def wrapper(*args, **kw):
        logger.info(f"{func.__name__} running...")
        start_at = time()
        try:
            return await func(*args, **kw)
        finally:
            logger.info(f"{func.__name__} end, cost {time() - start_at:.2f}s")

    return wrapper


@timer
def io_blocking_task():
    """I/O 型阻塞调用"""
    sleep(1)


@timer
def cpu_blocking_task():
    """CPU 型阻塞调用"""
    for _ in range(1 << 26):
        pass


@async_timer
async def coroutine_task():
    """异步协程调用"""
    await asleep(1)


@async_timer
async def coroutine_error():
    """会抛出异常的协程调用"""
    raise AttributeError("yo")


@async_timer
async def coroutine_main():
    ioloop = get_event_loop()
    r = await gather(
        coroutine_task(),
        coroutine_error(),
        ioloop.run_in_executor(thread_executor, io_blocking_task),
        ioloop.run_in_executor(process_executor, cpu_blocking_task),
        return_exceptions=True,
    )
    logger.info(f"coroutine_main got {r}")


@timer
def main():
    get_event_loop().run_until_complete(coroutine_main())


if __name__ == "__main__":
    main()
```

有一些较为普适的经验：

* I/O 越少越好，尽量在内存里完成
* 内存分配越少越好，尽量复用
* 变量尽可能少，gc 友好
* 尽量提高局部性
* 尽量用内建函数，不要轻率造轮子

下列方法如非瓶颈不要轻易用：

* 循环展开
* 内存对齐
* zero copy（mmap、sendfile）

此外，“静态化”是一种提高程序可读性和可维护性的重要手段，比如在函数定义时指明 type-hints，写清楚参数和返回值的类型。 以及对于 OOP，也可以写出定义明确的的“接口-实现”型代码，比如按照 `abc` -&gt; `BaseClass` -&gt; `Class` -&gt; `Instance` 的形式进行定义，就会规范很多。

```python
from abc import ABC, abstractmethod, abstractproperty

class ThingsABC(ABC):
    @abstractproperty
    def etable(self):
        pass


class BaseFood(ThingsABC):
    etable = True


class BirdABC(ABC):
    """
    在抽象类中定义抽象方法和属性，
    实例化的时候会自动检查这些抽象方法和方法必须已被实现，否则会抛出一场。

    具体实现的方法多种多样，比如直接在类里定义，或者多继承等等
    """
    @abstractmethod
    def fly(self):
        pass

    @abstractmethod
    def eat(self, food: BaseFood):
        pass


class BaseBird(BirdABC):
    """
    可以定义一些鸟类都应该有的通用属性和方法
    """
    pass


class Robin(BaseBird):
    """
    定义一些知更鸟特有的属性和方法
    """
#     def fly(self):
#         pass

#     def eat(self, foold: BaseFood):
#         pass


r = Robin()  # 会报错，因为没有实现抽象方法

# ---------------------------------------------------------------------------
# TypeError                                 Traceback (most recent call last)
# <ipython-input-1-a4984ec6275b> in <module>
#      45
#      46
# ---> 47 r = Robin()  # 会报错，因为没有实现抽象方法

# TypeError: Can't instantiate abstract class Robin with abstract methods eat, fly
```

