

精简的镜像的选择：



- scratch

  `scratch` 是一个空镜像，只能用于构建其他镜像，比如你要运行一个包含所有依赖的二进制文件，如 `Golang` 程序，可以直接使用 `scratch` 作为基础镜像

-  busybox

  BusyBox` 是一个集成了一百多个最常用 `Linux` 命令和工具（如 `cat`、`echo`、`grep`、`mount`、`telnet` 等）的精简工具箱，它只需要`几百KB` 的大小，很方便进行各种快速验证，被誉为 `Linux 系统的瑞士军刀

  `BusyBox` 可运行于多款 `POSIX` 环境的操作系统中，如 `Linux`(包括 `Android`)、`Hurd`、`FreeBSD` 等

- alpine 镜像

  `Alpine` 采用了 `musl libc` 和 `busybox` 以减小系统的体积和运行时资源消耗，但功能上比 `busybox` 又完善的多，`Alpine` 还提供了自己的包管理工具 `apk`，可以通过 [`packages`](https://pkgs.alpinelinux.org/packages)网站上查询包信息，也可以直接通过 apk 命令直接查询和安装各种软件。

  更改源下载会快一些。apk add方式安装

  ```
  # 修改源
  sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories
  ```

  

  https://www.jakehu.me/2019/docker-image/ from  镜像的选择