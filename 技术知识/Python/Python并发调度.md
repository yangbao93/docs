# Python并发调度

## 背景

python是非常方便的语言，在一些脚本执行上非常有优势。

最近遇到一个问题，需要刷一批数据，数据量很大，串行耗时很长，这时候需要并发进行刷数据，用python实现并发调用，并能合理的控制并发量，是需要学习的一个地方。

**方式就**

​	**线程池 or 协程** 

## 一、利用ThreadPoolExecutor开启并发调度

### 使用方法

1. 引入**ThreadPoolExecutor, as_completed** ，都是在concurrent.futures下；
2. 利用**with ThreadPoolExecutor**开启线程池；
3. 设置ThreadPoolExecutor的**max_workers**设置最大并发数量；
4. 利用submit方法提交执行的方法名，方法参数；

### 样例代码

```python
from concurrent.futures import ThreadPoolExecutor, as_completed
import time

NUMBERS = range(1, 100)

def fib(n):
    time.sleep(10)
    return n
  
# 开启线程池，最大线程数50
with ThreadPoolExecutor(max_workers=50) as executor:
    start = time.time()
    # executor中submit，submit(方法名，方法参数...): 参数名 for 参数名 in 参数list
    numbers_ = {executor.submit(fib, num): num for num in NUMBERS}
    # as_completed方法介绍在下面
    for task in as_completed(numbers_):
        # task_ = numbers_[task]
        # 输出返回结果
        print('result is {}'.format(task.result()))
    end = time.time()
    print('cost {}'.format(end - start))
```

> `as_completed()`方法是一个生成器，在没有任务完成的时候，会阻塞，在有某个任务完成的时候，会`yield`这个任务，就能执行for循环下面的语句，然后继续阻塞住，循环到所有的任务结束。从结果也可以看出，**先完成的任务会先通知主线程**。

### 执行结果

```
10并发下：cost 100.13678193092346
50并发下：cost 20.117856979370117
```

### 原理

目前简单运行，原理待补充

## 二、协程

待补充