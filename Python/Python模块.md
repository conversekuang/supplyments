

- 将工程中的依赖树可视化，pipdeptree 该方法能够全局安装在计算机上，或者virtualenv。

  ​	https://mp.weixin.qq.com/s/x5Bn8AS5yecRQY3aMXV4Tw





- py转换成exe

  Pyinstaller 将Py转换成exe文件。用虚拟环境可以将生成exe大小减少，

  ```
  # 创建虚拟环境的相关步骤
  conda create -n 虚拟环境名字 python==3.6  #创建虚拟环境
  conda activate 虚拟环境名字  #激活虚拟环境
  conda deactivate  #退出虚拟环境
  
  
  #创建虚拟环境
  conda create -n aotu python=3.6
  
  #激活虚拟环境
  conda activate aotu
  
  #Pyinstaller打包
  Pyinstaller -F -w -i apple.ico py_word.py
  ```

  https://mp.weixin.qq.com/s/4nuzZbgs7LaipMvZxvNn9w	





- exe转换成py

  https://mp.weixin.qq.com/s/9508u1IKCW541lyzwkzF_Q



