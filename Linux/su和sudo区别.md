



> su和sudo区别

**1. su： switch user的缩写切换用户**

- su <user_name>

  那么是一种 non-login-shell 的方式，意思是说我现在切换到了 <user_name>，但是当前的 shell 还是加载切换之前的那个用户的环境变量以及各种设置。

- su - <user_name>

  那么是一种 login-shell 的方式，意思是说切换到另一个用户 <user_name> 之后，当前的 shell 会加载 <user_name> 对应的环境变量和各种设置。

总结： 两种方式主要影响的是环境变量和设置。

（`su` 命令不跟任何 <user_name> ，**默认切换到 root 用户**）



**2. sudo 的英文全称是 super user do，即以超级用户（root 用户）的方式执行命令。**

```shell
ubuntu@VM-0-14-ubuntu:~$ tail -n 3 /etc/shadow
tail: cannot open '/etc/shadow' for reading: Permission denied      # 没有权限
ubuntu@VM-0-14-ubuntu:~$ sudo !!                                    # 跟两个惊叹号
sudo tail -n 3 /etc/shadow
ntp:*:17752:0:99999:7:::
mysql:!:18376:0:99999:7:::
test_user:$6$.ZY1lj4m$ii0x9CG8h.JHlh6zKbfBXRuolJmIDBHAd5eqhvW7lbUQXTRS//89jcuTzRilKqRkP8YbYW4VPxmTVHWRLYNGS/:18406:0:99999:7:::
ubuntu@VM-0-14-ubuntu:~$
```

我们使用了 `sudo !!` 这个小技巧，**表示重复上面输入的命令，只不过在命令最前面加上 sudo** 。



- 前者输入 `sudo su -` 后，需要提供当前用户的登录密码，也就是 ubuntu 用户的密码；
- 后者输入 `su -` 后，需要提供 root 用户的登录密码



**3. sudo 工作原理**

一个用户能否使用 `sudo` 命令，取决于 `/etc/sudoers` 文件的设置

​ /etc/sudoers 也是一个文本文件，但是因其有特定的语法，我们不要直接用 vim 或者 vi 来编辑它。

**需要用 visudo 直接编辑 /etc/sudoers 这个文件了。只有 root 用户有权限使用 visudo 命令**

```powershell
# User privilege specification
root    ALL=(ALL:ALL) ALL

# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "#include" directives:

#includedir /etc/sudoers.d
ubuntu  ALL=(ALL:ALL) NOPASSWD: ALL
```

- 第一个表示用户名，如 `root` 、`ubuntu` 等；
- 接下来等号左边的 `ALL` 表示**允许从任何主机登录当前的用户账户**；
- 等号右边的 `ALL` 表示：**这一行行首对一个的用户可以切换到系统中任何一个其它用户**；
- 行尾的 `ALL` 表示：**当前行首的用户，能以 root 用户的身份下达什么命令，`ALL` 表示可以下达任何命令**。



​	root用户才能新建用户 

```
useradd xxx # 新建用户 
passwd xxx  # 为新建用户创建密码
```







