

将原始的pcap按多少包进行分割

editpcap   src.pcap -F pcap -c 1000 out.pcap

```
默认转换出来的是pcapng文件，magic number和pcap的不一致
```



将原始的pcap按多少时间进行分割

editpcap   src.pcap -F pcap -i 30 out.pcap

```powershell
editcap.exe F:\jmx_8users_in_whereby\corr\第五次采集\sv3-cl3-5.pcap -F pcap -i 30 F:\jmx_8users_in_whereby\corr\第五次采集\out\server3-5.pcap
```

editcap.exe F:\filtering_dataset_results\---application--\vuvuzela_in_whereby_application_UDP







SplitCap -p 100000 -b 100000 -r {} -o {} -y L7

```
默认只能对pcap文件进行转换
```

-p 并行文件数量

-b  文件缓存字节数

-r 输入文件

-o 输出文件夹

-y 提取payload





