



https://programmerah.com/solved-minio-start-error-warning-console-endpoint-is-listening-on-a-dynamic-port-33669/

address 是内嵌网页 8000 

console-address 是api 9000 

两个端口都需要开放。



```python
from minio import Minio
import socket
import socks

if __name__ == '__main__':
    # 安装Pysocks库，脚本产生的socket都会经过该网口
    socks.set_default_proxy(socks.SOCKS5, "127.0.0.1", 9150)
    socket.socket = socks.socksocket

    # Minio只能用于HTTP代理，不能socks5代理
    minioClient = Minio(
        endpoint='49.4.114.46:9000',  # 文件服务地址
        access_key='minioadmin',  # 用户名
        secret_key='minioadmin',  # 密钥
        secure=False  # 设为True代表启用HTTPS
    )

    bucketname = "mybucket"
    buckets = minioClient.list_buckets()
    for bucket in buckets:
        print(bucket.name, bucket.creation_date)

    minioClient.fput_object(bucketname, "morning.pdf", "C1.pdf")
```



