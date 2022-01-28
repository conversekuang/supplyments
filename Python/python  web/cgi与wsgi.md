





web框架的部署有关系：





`CGI`:通用网关接口。

https://www.runoob.com/python3/python3-cgi-programming.html

https://www.jianshu.com/p/c4dc22699a42



**WSGI**:通信协议。是一种描述web服务器（如nginx，uWSGI等服务器）如何与web应用程序（如用Django、Flask框架写的程序）



**uWSGI**:是一个Web服务器，它实现了WSGI协议、uwsgi、http等协议。它要做的就是把HTTP协议转化成语言支持的网络协议。

- HTTP协议转化成WSGI协议，让Python可以直接使用。
- Nginx中HttpUwsgiModule的作用是与uWSGI服务器进行交换

uWSGI是实现了uwsgi和WSGI两种协议的Web服务器

<img src="https://images2018.cnblogs.com/blog/720785/201803/720785-20180315175024706-1983173921.jpg" style="zoom:50%">



**uwsgi**: **uwsgi协议是一个uWSGI服务器自有的协议**，它用于定义传输信息的类型（type of information），每一个uwsgi packet前4byte为传输信息类型描述，用于与nginx等代理服务器通信，它与WSGI相比是两样东西





python常用的

wsgi server: uwsgi gunicore，wsgiref,

asgi:  Uvicorn，Daphne 或 Hypercorn



---

`WSGI`（**Web服务器网关接口**）是基于`HTTP`协议模式的，不支持`WebSocket`，而`ASGI`的诞生则是为了解决`Python`常用的`WSGI`不支持当前`Web`开发中的一些新的协议标准。同时，`ASGI`对于`WSGI`原有的模式的支持和`WebSocket`的扩展，**即`ASGI`是`WSGI`的扩展**。



ASGI（**异步**服务器网关接口）是WSGI的精神继承者，旨在在具有异步功能的Python Web服务器，框架和应用程序之间提供标准接口。



支持异步的ASGI框架：

Starlette, Django Channels, Quart，FastAPI

FastAPI：FastApi 最主要的特点是快，非常高的性能，向 NodeJS 和 Go 看齐，现有最快的Python框架之一







https://www.cnblogs.com/2020-zhy-jzoj/p/13164861.html



https://www.cnblogs.com/yanjidong/articles/13198697.html



https://blog.csdn.net/zzddada/article/details/90747092

