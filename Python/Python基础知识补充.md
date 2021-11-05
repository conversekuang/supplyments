



- 多重继承情况下，子类的实例化，其实都是子类，只是从父类继承来的属性，不存在对父类实例的初始化：

  https://mp.weixin.qq.com/s/tc_M6oBGvCBiYY4mHCULTg



- 静态方法和类方法使用。

  一句话总结：静态方法就是某个类专用的工具函数。【和普通函数没有区别】

  一句话总结：当你想使用工厂模式，根据不同的参数生成同一个类的不同对象的时候，就可以使用类方法。

  【场景：当初始化输入不同参数，类方法都能转换成相同的`__init__`方法。】

  

  https://mp.weixin.qq.com/s/zNQ9rhsvTR5dT41AUibjBg



- Python抽象类的实现

  Python 支持抽象类。Python 自带的abc模块用于实现抽象类相关的定义和操作

  https://mp.weixin.qq.com/s/3ftY3Y9ngcAoWHMrKdqZdA

​		

- 在Python中使用类型标注

  当函数传入的参数是一个类，Pycharm无法得知该参数的类型，进而无法得到该类型的属性和方法。PY3使用类型标注，

  ```
  变量名: 类型 = 值
  def test(name: str, age: int=25, other_info: dict=None): 
  # test函数接收两个参数，第一个参数name是str类型，第二个参数age是int型并且默认值为25，并且第三个参数other_info是字典，默认值为None
      age = detail_info['age']
      
  class A:
  	def __init__(self):
  		self.name = "test"
  	
  	def walk(self):
  		print("I am walk")
  
  def run(x: A):
  	x.walk()
  ```

  https://mp.weixin.qq.com/s/jC23mdouEs9vw9DIQ7u3Qg





- 通过Python 3的类型标注提高PyCharm的自动补全能力	

  https://mp.weixin.qq.com/s/BJbDSmYAUQX4vBjLTMTTLA



- import 和from import的区别

  - 你只能导入一个模块或者导入一个函数或者类，你不能导入一个文件夹。

  - 无论你使用的是`import xxx`还是`from xxx.yyy.zzz.www import qqq`，你导入进来的东西，要不就是一个模块(对应到.py 文件的文件名)，或者是某个.py 文件中的函数名、类名、变量名。

  - 无论是`import xxx`还是`from xxx import yyy`，你导入进来的都不能是一个文件夹的名字。
  - 都禁止使用`from xxx import *`这种写法，它会给你带来无穷无尽的噩梦

  https://www.kingname.info/2020/03/23/know-import/



- `__init__.py`的作用

  - 精简导入路径：

    这是因为，当一个文件夹里面有`__init__.py`以后，这个文件夹就会被 Python 作为一个`包(package)`来处理。此时，对于这个包里面层级比较深的函数、常量、类，我们可以先把它们导入到`__init__.py`中。这样以来，包外面再想导入这些内容时，就可以用`from 包名 import 函数名`来导入了

  - 无视工作区的相对引用：

    为什么会有包这个东西呢？这是因为，当有一些代码会在很多地方被使用时，我们可以把这些代码打包起来，作为一个公共的部分提供给其他模块调用。由于调用包的其他模块所在的绝对路径是千变万化的，所以在包的内部调用自身其他文件中的函数、常量、类，就应该使用相对路径，而是绝对路径。


  当一个文件夹里面包含`__init__.py`时，这个文件夹会被 Python 认为是一个`包(package)`，此时，包内部的文件之间互相导入可以使用相对导入，并且通过提前把函数、常量、类导入到`__init__.py`中再在其他文件中导入，可以精简代码。

https://www.kingname.info/2020/03/23/init-in-python/



- Python函数重载	

  通过 `functools`模块里面的`singledispatch`装饰器实现函数重载。
  
  https://www.kingname.info/2019/12/11/singledispatch/



- 生成器

  生成器的新用法。

  ​	拆成多个模块，返回的是一个生成器，每次生成一个数据就会进行流水线处理。避免每次建立列表而集体返回。【之前都是直接for来读取生成器，没有返回并重新作为参数返回。值得学习该风格。】

  https://www.kingname.info/2019/10/30/you-should-learn-yield/



- Python 提醒  不能覆盖 某个父类方法

  Python3.8 原生就能做到final关键字。**无法禁止，但是可以提醒覆盖**。

  https://mp.weixin.qq.com/s/AlcOv2tmrwWhW58F36YYPA	



- Python 程序报错来禁止父类方法的覆盖。

  通过metaclass来实现。禁止子类的重写。

  https://mp.weixin.qq.com/s/PV7pjq5fXQbNDC1RPChM_Q





- **metaclass 原类**  

  https://zhuanlan.zhihu.com/p/98440398

  - 所有的 Python 的用户定义类，都是 type 这个类的实例

  - 用户自定义类，只不过是 type 类的 `__call__` 运算符重载

  - metaclass 是 type 的子类，通过替换 type 的 `__call__` 运算符重载机制，“超越变形”正常的类。

    *是new还是call？*

  ```
  查看源码，type也继承自object
  
  >>> type(object)
  <class 'type'>
  
  type为对象的顶点，所有对象都创建自type。
  object为类继承的顶点，所有类都继承自object。
  
  
  object是所有类的超类，而且type也是继承自object；所有对象创建自type，而且object也是type的实例。
  “type是object的类型，同时，object又是type的超类”，那到底是先有object还是先有type呢？这就像“先有鸡还是先有蛋问题”。
  object和type是python中的两个源对象，事实上，它们是互相依赖对方来定义，所以它们不能分开而论。
  通过这两个源对象可以繁育出一堆对象：list、tuple、dict等。元类就是这些类的类，这些类是元类的实例。
  ```



- for 用法。

  - 必须是iterable的对象。

  - for...else的用法

  https://mp.weixin.qq.com/s/Uo5nreXv77TXsSB5hBTzEQ