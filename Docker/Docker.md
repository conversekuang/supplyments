---

---



#### docker



![](https://mmbiz.qpic.cn/mmbiz_jpg/otNtibX96l9ibwMmqp4H0uRMU8ib3Rar6K6Tq2792RY1EBs7ibI50bXpz3KssjHmxsM6daT6ZribO47ibUCpjmUG97Ng/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



文章来自黄庆兵老师的网易蜂巢《玩转 Docker 镜像》系列https://github.com/bingohuang/play-docker-images，原本分为上下两篇，这里由生态君整合成一篇便于阅读。希望能对大家有所帮助。



![](https://mmbiz.qpic.cn/mmbiz_jpg/otNtibX96l9ibwMmqp4H0uRMU8ib3Rar6K6mMHg3xYCb7QCfotOVZNeBmiaZKLI1oVKHbibrQOna5cyMEV0GE3v53JA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



#删除所有未运行的容器（已经运行的删除不了，未运行的就一起被删除了）	

sudo docker rm $(sudo docker ps -aq)



Docker容器入门资料【全】

https://mp.weixin.qq.com/s/7BvtSbn3CwIsXoFnlQJR8Q




