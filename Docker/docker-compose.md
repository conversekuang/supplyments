

#### docker-compose 



https://xyzghio.xyz/DockerCompose/#more

实现了一条命令启动多个容器服务。



```
Docker Compose是一个用来定义和运行复杂应用的Docker工具。一个使用Docker容器的应用，通常由多个容器组成。使用Docker Compose不再需要使用shell脚本来启动容器。 Compose 通过一个配置文件来管理多个Docker容器，在配置文件中，所有的容器通过services来定义，然后使用docker-compose脚本来启动，停止和重启应用，和应用中的服务以及所有依赖服务的容器，非常适合组合使用多个容器进行开发的场景
	
用于启动N个镜像，一个项目中所需要用到的软件
```











docker-compose命令需要在docker-compose.yaml所在的目录下执行

[安装docker-compose](https://www.cnblogs.com/xiaochangwei/p/8204992.html) 





#### Makefile构建docker/docker-compose

​	[Makefile构建docker](https://www.cnblogs.com/woshimrf/p/make-docker.html)

​	Makefile是make命令的规则配置文件。

```
刚开始学习docker命令的时候，很喜欢一个字一个字敲，因为这样会记住命令。后来熟悉了之后，每次想要做一些操作的时候就不得不重复的输入以前的命令。当切换一个项目之后，又重复输入类似但又不完全相同的命令，仅仅通过history命令加速也有限。于是想，把要用的命令写到shell里，然后调用shell脚本去做。刚开始确实是这样做的。比如https://github.com/Ryan-Miao/docker-yapi。直到有一天，发现有人使用Makefile来存储操作，瞬间感觉很棒。
```

​	在所在文件路径下执行make xxx。xxx是Makefile中的存储的指令。可以通过的简单的make xxx来操作，代替复杂的指令。【当不熟悉命令的时候还是敲命令最好，方便熟悉命令】



在docker-compose中的Makefile，写make命令

```shell
all: build

CAPTURE_SERVICE=capture
URL_SERVICE=url_queue
CAPTURE_SCALE=8


# 这一句有两个问题，
#第一，进入到build_tcpdump_controller后直接执行的tcpdump_controller文件夹中makefile的build命令
#第二，这个$(SERVICE_NAME)是从哪儿出来的变量，是docker-compose.yaml的么？
build: build_tcpdump_controller
	docker-compose build $(SERVICE_NAME)
	
build_tcpdump_controller:
	# make -C： Change to DIRECTORY before doing anything.
	make -C tcpdump_controller

up: build
	# Run image
	docker-compose up -d --force-recreate --scale $(URL_SERVICE)=1 --scale $(CAPTURE_SERVICE)=1

scale: build
	# Run image
	docker-compose up -d --force-recreate --scale $(URL_SERVICE)=1 --scale $(CAPTURE_SERVICE)=$(CAPTURE_SCALE)
logs:
	docker-compose logs -f
down:
	docker-compose down
```



​	

