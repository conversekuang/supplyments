

在docker中运行mysql数据库



mysql  8.0



开放宿主机的3306端口，新建挂载文件夹路径

```
sudo docker run -p 3306:3306 --name mysql_own \
-v /usr/local/docker/mysql/conf:/etc/mysql \
-v /usr/local/docker/mysql/logs:/var/log/mysql \
-v /usr/local/docker/mysql/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:latest
```



遇见navicat 遇到1251错误的时候

```
GRANT ALL ON *.* TO 'root'@'%';
ALTER USER 'root'@'%' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER;
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'root';
flush privileges;
```



https://blog.csdn.net/a15123837995/article/details/83751612



mysql 5.7

```
sudo docker run -itd -p 33060:3306 --name my-mysql -v /space/mysql_docker_dir/conf:/etc/mysql -v /space/mysql_docker_dir/logs:/var/log/mysql -v /space/mysql_docker_dir/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7
```



---

##### sysbench对mysql进行性能测试



apt install sysbench

```
sysbench oltp_read_write.lua --mysql-host=127.0.0.1 --mysql-port=33306 --mysql-db=sbtest --mysql-user=root --mysql-password=root --table_size=5000 --tables=10 --threads=2 --time=60 --report-interval=10 --db-driver=mysql  prepare

sysbench oltp_read_write.lua --mysql-host=127.0.0.1 --mysql-port=33306 --mysql-db=sbtest --mysql-user=root --mysql-password=root --table_size=5000 --tables=10 --threads=2 --time=60 --report-interval=10 --db-driver=mysql  run

sysbench oltp_read_write.lua --mysql-host=127.0.0.1 --mysql-port=33306 --mysql-db=sbtest --mysql-user=root --mysql-password=root --table_size=5000 --tables=10 --threads=2 --time=60 --report-interval=10 --db-driver=mysql  cleanup
```



此处的mysql是在docker中安装的

https://www.cnblogs.com/rongfengliang/p/7889551.html



