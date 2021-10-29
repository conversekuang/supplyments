---

---



#### docker



![](https://mmbiz.qpic.cn/mmbiz_jpg/otNtibX96l9ibwMmqp4H0uRMU8ib3Rar6K6Tq2792RY1EBs7ibI50bXpz3KssjHmxsM6daT6ZribO47ibUCpjmUG97Ng/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



文章来自黄庆兵老师的网易蜂巢《玩转 Docker 镜像》系列https://github.com/bingohuang/play-docker-images，原本分为上下两篇，这里由生态君整合成一篇便于阅读。希望能对大家有所帮助。



![](https://mmbiz.qpic.cn/mmbiz_jpg/otNtibX96l9ibwMmqp4H0uRMU8ib3Rar6K6mMHg3xYCb7QCfotOVZNeBmiaZKLI1oVKHbibrQOna5cyMEV0GE3v53JA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

















\#删除所有未运行的容器（已经运行的删除不了，未运行的就一起被删除了）	

sudo docker rm $(sudo docker ps -aq)



#### Dockerfile

​	Dockerfile制作镜像时候，cargo build 需要的获取的是依赖库但是cargo源不好，在dockerfile中更新源，加入COPY 文件到镜像中。将更换的源文件名称为**config**。之前一直命名为config.toml了。导致错误

​	制作镜像的时候都加上就不会出现问题。



​	[Dockerfile的入坑路](https://blog.csdn.net/qq_39629343/article/details/81561715)

​	[解决Rust -- update crates.io过慢的问题](https://learnku.com/articles/49977) ，是config

​	[Dockerfile 构建镜像发布web](https://www.cnblogs.com/xiaochangwei/p/8204992.html)

​	



Docker-machine 又是管理docker 的，那么这三个有什么区别？

​	

docker

 https://www.bilibili.com/read/cv5907039

在docker中添加权限，相当于sudo

```bash
RUN echo "${DEFAULT_USER} ALL=(ALL) NOPASSWD" >> /etc/sudoers.d/tcpdump
```



在copy进入image之前，将源文件的chmod改变。

```
RUN python3 -m virtualenv -p python3 env && \
    install_python_dependencies.sh env requirements.txt && \
	rm -f install_python_dependencies.sh  requirements.txt && \
	mkdir src

```









- - 
  







---

