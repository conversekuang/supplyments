





socks在定义中是在传输层和应用层之间。可被视作传输层。

socks5协议在socks4协议基础之上扩展了udp代理等功能。SOCKS5可用接受TCP和UDP

```
The protocol is conceptually a "shim-layer" between the application layer and the transport layer
```





socks5协议

[socks5 协议详解 - Jiajun的编程随想 (jiajunhuang.com)](https://jiajunhuang.com/articles/2019_06_06-socks5.md.html)

[理解socks5协议的工作过程和协议细节 | Bigbyto (wiyi.org)](https://wiyi.org/socks5-protocol-in-deep.html)

[Socks5 udp代理 - 简书 (jianshu.com)](https://www.jianshu.com/p/cf88c619ee5c)

[Socks5协议简介 - 简书 (jianshu.com)](https://www.jianshu.com/p/e44a149610ae)



### TCP代理：



整个协议在TCP连接连接之后，到真实发数据之前，加了一些协议过程。

<center><img src = "https://i.loli.net/2021/11/26/PRZWyloDv7SNaEX.png"></center>

其中红色部分为socks5的协议部分。

因为红色之前是三次握手，之后就是TLS握手了。所以是再建立TCP连接之上。



下面是网络抓包的 **握手 + 请求阶段**，如果请求阶段需要认证则需要再次的认证阶段。

#### 握手

客户端   ----->  服务器端  发送3字节

![image-20211126153522942](https://i.loli.net/2021/11/26/1wOkcX5qEKZDFmu.png)





服务器端   ----->  客户端  发送2字节

![image-20211126153539296](https://i.loli.net/2021/11/26/1qZykrah3C5NDn8.png)



#### 请求

客户端   ----->  服务器端  发送23字节

![image-20211126153448921](https://i.loli.net/2021/11/26/UI3DLoqhCersNw7.png)





服务器端   ----->  客户端  发送10字节

![image-20211126153613166](https://i.loli.net/2021/11/26/iHsx7CYN84kGbcO.png)





### UDP代理：



UDP详细

[Socks5代理分析 - 贵大头的博客 (guiyongdong.github.io)](https://guiyongdong.github.io/2017/12/09/Socks5代理分析/)

​	

​	相同的UDP payload，客户端发送到代理端会多10字节，是协议的头部。代理端发送到目的端，会将协议头去掉，还原出来payload。

​	

​	这个实际交换数据的UDP的端口不是协议上的端口。因为当客户端知道socks5的UDP代理服务器地址和端口后，是往这个端口上进行通信的。

​	





​	可以发现，udp代理的建立是在与代理服先建立tcp连接，tcp连接上先“握手”和“准备代理”，客户端知道udp代理地址后，就不用这个tcp连接了，直接udp代理了，理论上这个tcp连接无用了，但socks5协议指出，这个tcp连接要保持长连接，如果断开，则相应的udp代理也要结束。

​	

gost设置UDP over TCP

https://v2.gost.run/socks/



---



​	