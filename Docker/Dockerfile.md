## Dockerfile 制作镜像





### 基础知识



#### Dockerfile















###  资料



- docker镜像制作【七步制作精简镜像，So easy】from  **Go编程时光**

​	https://mp.weixin.qq.com/s/BKvcgM2h1tOlAQXnIfgQfA



- Dockerfile 入门 from zhu

  https://xyzghio.xyz/DockerBuildImage/#more



### 疑惑解答



> 为什么再nginx的Dockerfile 中需要CMD [ "nginx", "-g", "daemn off;"]
>
> https://blog.csdn.net/youcijibi/article/details/88781014

​	因为docker容器再执行过程中会判断pid=1的程序是否再执行，如果pid=1的程序执行完毕，那么容器就会关闭。因此，会出现运行程序直接结束的情况。所以要nginx再前台执行。









### 问题



问题汇总：

- [docker启动报错：standard_init_linux.go:211: exec user process caused "no such file or directory"](https://blog.csdn.net/feinifi/article/details/102910715)
  - vim中将set ff=unix 格式改变一下

- docker启动：| standard_init_linux.go:211: exec user process caused "permission denied"
  - 直接将源文件chmod 755 然后重新build



​	[Dockerfile的入坑路](https://blog.csdn.net/qq_39629343/article/details/81561715)

​	[解决Rust -- update crates.io过慢的问题](https://learnku.com/articles/49977) ，是config

​	[Dockerfile 构建镜像发布web](https://www.cnblogs.com/xiaochangwei/p/8204992.html)

​	

