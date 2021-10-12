

awk：

- awk ' BEGIN {statement}  {statement} END {statement} '

- awk ' BEGIN {statement}  {statement} END {statement} ' filename



```
NR:行数
NF:列数
$0:当前行内容
$1:分隔符后的第一个
$(NF)：分隔符最后一个字段
```

awk -F:   修改分隔符，默认是空格。可以修改成其他。





> 批量修改linux后缀名

ll | awk '{split($9,a,".");print a[1]}' |xargs -i mv {}.pkl {}.pickle

