

从 Github 上 Clone 仓库经常网络不给力，怎么办？

使用镜像站：https://hub.fastgit.org

比如你要 clone 该仓库

```
$ git clone https://github.com/iswbm/magic-python.git
```

可以换成这个

```
$ git clone https://hub.fastgit.org/iswbm/magic-python.git
```







##### 5. go get 镜像源

使用 Go 的朋友都知道，go get 安装包都是从 github 下载的，可以执行如下命令为其配置一个镜像网站

```
go env -w GOPROXY=https://goproxy.cn,direct
```

常用的镜像源有下面三种，你选一种即可：

- https://goproxy.io
- https://goproxy.cn
- https://mirrors.aliyun.com/goproxy/
- 