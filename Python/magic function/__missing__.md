



- dict 字典 实现 `__missing__()` 魔术方法，能够实现当Key不存在时候不报错，返回一个值。

  源码中并有该magic method

  d[‘a’] 存在，那么只调用 __getitem__ 就结束了，
  d[‘b’] 不存在，那么__getitem__会失败，之后就会尝试调用 __missing__ 方法。

  https://www.kingname.info/2019/11/30/quite-dict/

