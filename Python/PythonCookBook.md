







2.3 字符串匹配

- fnmatch：底层系统大小写敏感和规则。不同系统不一样。
- fnmatchcase：它完全使用你的模式大小写匹配



2.4 字符串匹配与搜索

- str.startswith

- str.endswith

- str.find(text) 返回text在str中的location index

  搜索



---



4.1 手动遍历

​	对可迭代对象遍历且不使用for

- 通过next()获取每次对象，用完抛出`StopIteration`。**next(可迭代对象, None)**，没有东西的时候，返回None，不抛出异常



4.2代理迭代

​	你构建了一个自定义容器对象，里面包含有列表、元组或其他可迭代对象。 你想直接在你的这个新容器对象上执行迭代操作。

- 实现`__iter__()`函数。

  这里的 iter() 函数的使用简化了代码， iter(s) 只是简单的通过调用 `s.__iter__()` 方法来返回对应的迭代器对象

  

4.3 使用生成器生成新的迭代模式

- 一个函数中需要有一个 yield 语句，即可将其转换为一个生成器。
- 通过next进行获取



4.4 实现迭代协议

你想构建一个能支持迭代操作的自定义对象，并希望找到一个能实现迭代协议的简单方法。

- Python的迭代协议要求一个` __iter__() `方法返回一个特殊的迭代器对象， 这个迭代器对象实现了`__next__()` 方法并通过 StopIteration 异常标识迭代的完成。 但是，实现这些通常会比较繁琐。

- 简单的方法：将你的迭代器定义为一个生成器后一切迎刃而解。yield方式

- yield 和yield from，yield from可以代替内部for循环。

  

4.5 反向迭代

你想反方向迭代一个序列

- reversed()。反向迭代仅仅当对象的大小可预先确定或者对象实现了 `__reversed__()` 的特殊方法时才能生效

- 自定义类上实现 `__reversed__()` 方法来实现反向迭代。







4.7迭代器切片

- 函数 itertools.islice() 正好适用于在迭代器和生成器上做切片操作。
- 迭代器事先不知道大小，所以无法像正常切片使用。
- 原理是islice() 遍历到索引位置，把之前的丢弃，需要的返回。
- 迭代器是不可逆的，消耗的元素就无法再用。



4.8 跳过可迭代对象的开始部分

- itertools.dropwhile()，思路都是丢弃之前的。

```python
>>> from itertools import dropwhile
>>> with open('/etc/passwd') as f:
...     for line in dropwhile(lambda line: not line.startswith('#'), f):
...         print(line, end='')
```



4.9 排列组合输出

- 排列：itertools.permutations()
- 组合不可重用：itertools.combinations()
- 组合可重用：itertools.combinations_with_replacement()





4.10序列上索引值迭代

- 内置的 enumerate() 函数，有索引和对应的值



4.11同时迭代多个序列

- zip()每次从序列中取一个
- 迭代器的长度和zip参数中的**最短长度一致**
- itertools.zip_longest()，按zip参数中的**最长长度一致**



4.12 不同集合上元素的迭代

你想在多个对象执行相同的操作，但是这些对象在不同的容器中，你希望代码在不失可读性的情况下避免写重复的循环。

- **from itertools import chain**,可以将多个容器合并一起执行操作。且省存，相比于a+b合并【a+b操作必须是同类型的，tuple和list就不能合并，set和list也不能合并】。

- chain就不存在

```python
from itertools import chain
def print_iteral(*arr):
    for item in chain(*arr):
        print(item)
        
x=[1,2,3,4,5]
y=['z','b','xv']
w = {2,3,4}
print_iteral(x,y,w)
```



4.13 创建数据处理通道

你想以数据管道(类似Unix管道)的方式迭代处理数据。 比如，**你有个大量的数据需要处理，但是不能将它们一次性放入内存中**。

- 通过yield得到一组数据流

- yield是数据生产者，for循环是数据的消费者
- sum()函数是最终的程序驱动者，每次从生成器管道中提取出一个元素。

```python
lognames = gen_find('access-log*', 'www')
files = gen_opener(lognames)
lines = gen_concatenate(files)
pylines = gen_grep('(?i)python', lines)
bytecolumn = (line.rsplit(None,1)[1] for line in pylines)
bytes = (int(x) for x in bytecolumn if x != '-')
print('Total', sum(bytes))
```



4.14 展开嵌套的序列

- 你想将一个多层嵌套的序列展开成一个单层列表

- 语句 yield from 在你想在生成器中调用其他生成器作为子例程的时候非常有用。 如果你不使用它的话，那么就必须写额外的 for 循环了。
- str类型和bytes类型都是可迭代的，但是并不希望对他进行展开

- Iterable是collections里面的



4.15 顺序迭代合并后的排序迭代对象

- 首先输入是已经排好序的，a, b
- import heapq   heapq.merge(a, b) 只负责每次取序列开始最小的并返回。



4.16  迭代器代替while无限循环

- iter 函数一个鲜为人知的特性是它接受一个可选的 callable 对象和一个标记(结尾)值作为输入参数。 当以这种方式使用的时候，它会创建一个迭代器， 这个迭代器会不断调用 callable 对象直到返回值和标记值相等为止。



---

可迭代对象vs迭代器

迭代器：实现`__iter__`和`__next__`

迭代对象实现：`__iter__`或`__getitem__`



---

