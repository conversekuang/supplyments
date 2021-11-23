





> conda 与anaconda的区别

conda是包管理工具。

​	https://docs.conda.io/projects/conda/en/latest/index.html

Anaconda：是一套包括conda，numpy，scipy，ipython等等的一百个软件包。

Miniconda：它是Anaconda的一个小型替代品，它只是conda及其依赖项。



Python 多版本管理和环境依赖 、

conda

 conda安装 https://www.jianshu.com/p/edaa744ea47d

```
官网找到对应的下载版本 https://docs.conda.io/en/latest/miniconda.html

1. wget -c https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
2. chmod 777 Miniconda3-latest-Linux-x86_64.sh #给执行权限
3. bash Miniconda3-latest-Linux-x86_64.sh #运行

4. conda 命令无法执行的时候，编辑环境变量 vim ~/.bashrc
   export PATH=$PATH:path/to/conda/bin
```



本地的conda镜像 https://www.jianshu.com/p/95ecb6edd93e



conda 常用命令： https://www.cnblogs.com/sddai/p/10322603.html

​								https://www.jianshu.com/p/62774d72e624



- 管理通道

  conda config --get channels 或者 conda config --show channels   显示通道

  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/   添加通道

  conda config --remove channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/  删除通道



-  conda新建环境

  conda create --name py310 python=3.10  新建环境py310 指定py版本

  conda create --name xxx --clone yyy   将xxx克隆为yyy

  conda info --envs   目前为止已经安装了哪些环境



- 切换环境

​		conda activate xxx   /  source activate xxx     激活环境

​		conda deactivate     退出环境



- 导出依赖关系：

  conda env export --file ehbio_env.yml --name ehbio  \# 假设我们有一个环境叫 ehbio，可以导出为一个yml文件

  conda env create -n ehbio --file ehbio_env.yml   \# 然后换一台电脑，就可以完全重现这个环境了

  

- 移除依赖

  conda remove --name xxx  certain_pkg  将certain_pkg依赖从环境移除

  conda remove --name xxx --all   将xxx的所有依赖从环境移除



- 在创建的环境里面安装包依赖

  conda install  mmm 安装包依赖

> 当出现conda install 无法安装最新依赖模块的版本时候，需要pip进行安装。



conda install 和pip install的区别。

conda list  与 pip  list的区别

https://blog.csdn.net/qq_41368074/article/details/107783636



两者安装的路径是不一样的。

虚拟环境py3.8中安装在：所在路径为：/root/miniconda3/envs/py38/lib/python3.8/site-packages

conda安装在：/root/miniconda3/pkgs



创建环境初始化完成后，发现pip list的结果少于conda list。condalist 

![image-20211123145218370](https://i.loli.net/2021/11/23/mhP28QiqnaGOItW.png)



在虚拟环境中通过pip安装包之后。conda list也能够查到

![image-20211123145501061](https://i.loli.net/2021/11/23/YTChLiJxNRrbSMK.png)





