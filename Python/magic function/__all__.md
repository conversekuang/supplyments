

`__all__`变量是一个特殊的变量，通常是一个列表，可以暴漏出来或者为了限制或者指定能被导入到别的模块的函数，类，全局变量等。



在模块 from xx import *时，通过 `__all__` 来指定可以导入的模块，其他的模块不会导入。

只有使用`from xx import *`该语句时候，才有效。


