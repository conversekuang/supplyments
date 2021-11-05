



Python 中存在很多魔法函数，对常见的和遇到的进行记录:





- `__str__`和`__repr__`

  print输出的是`__str__`，而debug模式下输出的是**字符串原始的模样**，也就是`__repr__`。

  如果所见即所得的话字符串用r''的方式。

  ```python
  >>> b = 'D:\game\pal4' 
  >>> b 
  'D:\\game\\pal4' 
  
  >>> print(repr(b)) 
  'D:\\game\\pal4' 
  
  >>> print(b) 
  D:\game\pal4 
  实际上，输入变量名，回车以后，你看到的才是这个字符串真正的样子，因为在Python里面是不存在单根反斜杠的。当你要表示反斜杠本身的时候，就应该是\\这种写法。
  
  当然在定义的时候你可以只写单根反斜杠，在大多数情况下，Python会理解你的意图，所以它会自动把单根反斜杠转换为两个反斜杠。
  ```

  而使用print关键字打印出来的，是经过Python优化，更便于人类阅读的样子
  
  https://mp.weixin.qq.com/s/V6o54zmoVByJMnPN0zlgpA





- dict 字典 实现 `__missing__()` 魔术方法，能够实现当Key不存在时候不报错，返回一个值。

  源码中并有该magic method

  d[‘a’] 存在，那么只调用 __getitem__ 就结束了，
  d[‘b’] 不存在，那么__getitem__会失败，之后就会尝试调用 __missing__ 方法。

  https://www.kingname.info/2019/11/30/quite-dict/

