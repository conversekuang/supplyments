



用户态 tcpdump 如何实现抓到内核网络包的?

https://mp.weixin.qq.com/s/reC7Hw5T-2waw-017jgv9Q









convert_pcap_to_log.py代替shell脚本，将采集的pcap转换成log日志文件

```python
def convert(srcpath, dstpath):
    for dirname in os.listdir(srcpath):
        srcdirpath = os.path.join(os.path.abspath(srcpath), dirname)
        dstdirpath = os.path.join(os.path.abspath(dstpath), dirname)
        if not os.path.exists(dstdirpath):
            os.mkdir(dstdirpath)
        for filename in os.listdir(srcdirpath):
            srcfilepath = os.path.join(srcdirpath, filename)
            filename = filename.replace("pcap", "log")
            dstfilepath = os.path.join(dstdirpath, filename)
            cmd = "tcpdump -i any -n -tt -q '(((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)' -r  {} > {}".format(srcfilepath, dstfilepath)
            return_val = os.system(cmd)
            if return_val != 0:
                raise Exception("error: {}".format(return_val))

    print("finished converting pcap to log")


if __name__ == '__main__':
    srcpath = sys.argv[1]
    dstpath = sys.argv[2]
    convert(srcpath, dstpath)
```



与上述脚本功能一致

```
#! /bin/bash
function read_dir(){
for file in `ls $1` #注意此处这是两个反引号，表示运行系统命令
do
 if [ -d $1"/"$file ] #注意此处之间一定要加上空格，否则会报错
 then
 read_dir $1"/"$file
 else
 tcpdump -i any -n -tt -q '(((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)' -r  $1"/"$file >> "./logs/$(basename ${file}).log"      #在此处处理文件即可
 fi
done
} 
#读取第一个参数
read_dir $1

'''
该文件夹是UDP寄生流抓取的pcap的情况
但是作为关联分析：
1. 解析pcap
2. 将pcap 转换成log，直接提取关键字。
此处是将pcap转换成log。

pcap里面放的是待转换的文件
执行脚本
./new_test.sh ./pcap
'''
```





