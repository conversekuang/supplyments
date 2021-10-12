

**#!** 是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种 Shell。



脚本传参：

./test.sh kzk  hxj

​	$0 代表的运行文件名本身， $1 之后依次代表参数本身。

​	$10 不能获取第十个参数，获取第十个参数需要${10}。当n>=10时，需要使用${n}来获取参数

$# 传递到脚本的参数个数

$*  以一个单字符串显示所有向脚本传递的参数。

$@ 与$*相同。【但是使用时加引号，并在引号中返回每个参数】

```
converse@ubuntu:~$ ./test.sh  kzk  hxj
$0 is :./test.sh
$1 is :kzk
$2 is :hjx
$@ the number of parameters:kzk hjx
$# each of parameters :2
```



变量：

**变量名和等号之间不能有空格**

```
your_name="runoob.com"
```

**使用变量的时候才加美元符（$）**

$your_name  或者 ${your_name}



字符串：

**单引号**的限制：

- 单引号里的任何字符都会原样输出，**单引号字符串中的变量是无效**；
- 单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。



**双引号**的优点：

- **双引号里可以有变量**
- **双引号里可以出现转义字符**



**拼接**：

字符串拼接不需要任何符号（包括+），直接变量和str进行结合。

```shell
your_name="runoob"
# 使用双引号拼接
greeting="hello, "$your_name" !"
greeting_1="hello, ${your_name} !"
echo $greeting  $greeting_1
输出结果为：
hello, runoob ! hello, runoob !

# 使用单引号拼接
greeting_2='hello, '$your_name' !'
greeting_3='hello, ${your_name} !'
echo $greeting_2  $greeting_3
输出结果为：
hello, runoob ! hello, ${your_name} !
```



字符串长度：

${#your_name}    6



 

数组

bash支持一维数组（**不支持多维数组**），并且没有限定数组的大小

用**括号来表示数组**，数组元素用"空格"符号分割开

- 数组中的某个下标的数值：${array_name[下标]}

- 获取数组中的所有元素：${array_name[@]}
- 获取数组长度：${#array_name[@]}  ， ${#array_name[*]}



**` 反引号**

expr 是一款表达式计算工具，使用它能完成表达式的求值操作。

完整的表达式要被 **\` `** 包含





条件控制：

```
if condition1
then
    command1
elif condition2 
then 
    command2
else
    commandN
fi
```

​	末尾的 fi 就是 if 倒过来拼写

**for**

```
for var in item1 item2 ... itemN
do
    command1
    ...
    commandN
done
```



**while**

```
while condition
do
    command
done
```



**case**

```
case 值 in
模式1)
    command1
    ;;
模式2）
    commandN
    ;;
esac
```

取值后面必须为单词 **in**

- 每一模式必须以右括号结束
- 命令开始执行直至 **;;**
- 一旦模式匹配，则执行完匹配模式相应命令后不再继续其他模式。
- 无一匹配模式，使用星号 * 捕获该值，再执行后面的命令

```
#!/bin/sh
site="runoob"
case "$site" in
   "runoob") echo "菜鸟教程"
   ;;
   "google") echo "Google 搜索"
   ;;
   "taobao") echo "淘宝网"
   ;;
   *)  echo 'unknown'
   ;;
esac
```





**脚本中运行其他文件**：

```shell
. filename   # 注意点号(.)和文件名中间有一空格
或
source filename

eg
. ./test1.sh
source ./test1.sh
```