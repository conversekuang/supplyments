

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





> 脚本输入服务器密码传输

启动tcpdump

sshpass -p  'xxxx' ssh dev@49.4.114.46 '/space/minio_dir/start_tcpdump_cmd.sh'

```shell
start_tcpdump_cmd.sh

sudo nohup tcpdump -i any -n -tt -q port 8000  and '(((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)' > /space/minio_dir/S2.log 2>&1 &
```



关闭tcpdump

sshpass -p  'xxxx' ssh dev@49.4.114.46 '/space/minio_dir/stop_tcpdump_cmd.sh'

```shell
stop_tcpdump_cmd.sh

ps -ef|grep tcpdump| grep -v grep|awk '{print $2}' |sudo xargs kill -9
```





