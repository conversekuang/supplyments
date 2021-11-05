





1. Python `__init__` 能不能单独调用？

   ```python
   class A(object):
       def __init__(self, name,age):
           print("init method")
           self.name = name
           self.age = age
   
       def __new__(cls, *args, **kwargs):
           print("new method")
           return object.__new__(cls)
   
   
   if __name__ == '__main__':
       a = A("a",50)
       print(a, a.name)
       a.name = "kzk"
       print(a, a.name)
       a.__init__("b",40)
       print(a, a.name)
   ###########################################
   new method
   init method
   <__main__.A object at 0x0000024A34B53910> a
   <__main__.A object at 0x0000024A34B53910> kzk
   init method
   <__main__.A object at 0x0000024A34B53910> b
   ```



​		new对象的时候，先`__new__`再`__init__`。

​		重写了new方法后，如果需要返回生成的对象，需要调用`object.__new__()`进行初始化。

​		`__init__`作为方法，可以单独进行调用，不一定必须通过`__new__`。比如对已经存在的对象重新初始化。

​		



2. function和method什么区别？

   **function是直接定义的函数**，就是函数。

   **method是在类中定义的**，不论是staticmethod，classmethod还是对象的普通函数，都是方法。

   Pycharm中会提示是f还是m。

   magic function是也是method，不是function。



3. 类可以直接调用实例方法？

​	http://c.biancheng.net/view/2267.html

​	个人感觉这个地方是个trick。一般我们在该类传入的都是该类的对象self，这里是通过传入别的实力对象但是同样命名为self。造成的一个假象。

```python
class CLanguage:
    def info(self, x):
        print(self, x, "正在学 Python")
        print(type(self))


# 通过类名直接调用实例方法
CLanguage.info(A("x",50),"a")
```

​	这里的self其实传入的是A()的实力对象。有点偷换概念的意思，虽然也可以运行。叫做偷换self。

​	总的来说，Python 中允许使用类名直接调用实例方法，但必须手动为该方法的第一个 self 参数传递参数，这种调用方法的方式被称为“非绑定方法”。

> 用类的实例对象访问类成员的方式称为绑定方法，而用类名调用类成员的方式称为非绑定方法。



4. `__call__`和`__new__`不是同时生效的。

   ```python
   class A(object):
       def __init__(self, name):
           print("__init__", name)
   
       def __call__(self,name):
           print("__call__", name)
   
   if __name__ == '__main__':
       a = A("kzk")
       A("s")
       a("w")
   ####################
   __init__ kzk
   __init__ s
   __call__ w
   ```

   