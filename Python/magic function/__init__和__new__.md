





在object基类中的定义。

`__init__`是初始化self实例对象。

![image-20220110095445917](https://s2.loli.net/2022/01/10/UepQEWNtsaFqmdT.png)



`__new__`是一个静态方法。创建并返回一个新的实例对象。

![image-20220110095412685](https://s2.loli.net/2022/01/10/SlY1tdhygfKxUQ5.png)









---



```python
class A:
    def __init__(self):
        self.x = 0

class B(A):
    def __init__(self):
        super().__init__()
        self.y = 1
```



1. `super().__init__()`



----

`__new__`方法的几种写法。

```python
class A:
    def __init__(self,name):
        print("__init__")
        self.name = name
    
    def __new__(cls, name):
        print("__new__")
        return object.__new__(A)

a = A("kzk")
a.name

#=================================#

__new__
__init__
'kzk'


#=================================#
class B:
    def __init__(self,name):
        print("__init__")
        self.name = name
    
    def __new__(cls, name):
        print("__new__")
        self = super(B,cls).__new__(cls)
        return self

b = B("kzk")
# b.name

__new__
__init__
'kzk'
```



书写格式可以表示为：

1. `object.__new__(A)` 或者 `object.__new__(cls)`

2. `super(B,cls).__new__(cls)`



> 问题1：`__new__`执行一定会执行`__init__`?  
>
> 错误。



```python
>>> d = Date.__new__(Date)
>>> d
<__main__.Date object at 0x1006716d0>
>>> d.year
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
AttributeError: 'Date' object has no attribute 'year'
```



- 例如：

```python
class C:
    def __init__(self,name):
        print("__init__")
        self.name = name
    
    def __new__(cls, name):
        print("__new__")
        self = super().__new__(cls)
        return self

c = C("kzkc")

#===================#
__new__
__init__
#===================#

class C:
    def __init__(self,name):
        print("__init__")
        self.name = name
    
    def __new__(cls, name):
        print("__new__")
        self = super().__new__(cls)
#         return self

c = C("kzkc")

#===================#
__new__
#===================#
```

`__new__`只要不返回实例，就不会进行初始化。

从结果可以看到，如果不返回实例，那么`__init__`就无法调用。



- 创建不调用init方法的实例

  你想创建一个实例，但是希望绕过执行 __init__() 方法。通过new 方法创建未初始化的实例。

```python
class A:
    def __init__(self,a):
        self.init = a
    
    @classmethod
    def _new_instance(cls):
        x = cls.__new__(cls)
        x.val = "xx"
        x.age = 10
        return x
    
a = A._new_instance()
b = A("b")

print(dir(a))   
print(dir(b))

#==================================#
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_new_instance', 'age', 'val']
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_new_instance', 'init']
```



所以创建一个对象不一定会调用`__init__()`方法



关于python源对象object的__new__()，为什么是静态方法？

https://www.imooc.com/wenda/detail/514113



http://cn.voidcc.com/question/p-cpznjppx-rr.html



python cookbook

https://python3-cookbook.readthedocs.io/zh_CN/latest/c08/p17_create_instance_without_invoking_init_method.html?highlight=__new__