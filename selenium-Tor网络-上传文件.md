

selenium4.0.0 + python3.8 + firefox 93.0 + geckodriver-v0.30.0 



20211103

为了自动化了访问Tor网络，

- 方案1：selenium直接操作Tor浏览器进行操作。https://github.com/webfp/tor-browser-selenium
- 方案2：selenium操作firefox，但是走Tor的浏览器代理9150端口socks5类型。

方案2，更加便捷。



> Tor浏览器

​	tor-browser-linux64-10.5.8_en-US.tar.xz



> selenium 4.0.0 版本	

​	下载地址：pip install selenium==4.0.0

​	问题：版本低，需要重新安装pyOpenSSL-21.0.0



> geckodriver

​	根据版本匹配firefox驱动。选择了最新的。

​	Geckodriver Selenium Firefox之间的版本适配问题

​	https://firefox-source-docs.mozilla.org/testing/geckodriver/Support.html

​	

​	Geckodriver的版本信息

​	https://github.com/mozilla/geckodriver/tags



需要确定各版本是否匹配的问题。



遇到问题：

1. webdriver启动需要加入proxy，类型为socks5.

   查看官方文档以及case。https://www.selenium.dev/documentation/webdriver/http_proxies/

   结合例子查看源码，成功为firefox设置Tor代理。

   

2. selenium语法问题

   很多方法过期，需要替换新的方式

   - webdriver输入类型变成service类型，不再是geckodriver path。

   - find_element只需要定位到节点即可，如：p，h1。返回的是WebElement类型，再提取text属性即可

   - 加载类型不同，需要采用wait方法直到出现。

   selenium官方文档：

​		https://www.selenium.dev/documentation/

​		

​		selenium with python的使用方式：

​		https://selenium-python.readthedocs.io/index.html



3. 可以采用不显示浏览器的方式。

   options加入 --headless

   https://pythonbasics.org/selenium-firefox/



**Linux selenium访问tor的**

```python
from selenium.webdriver.firefox.options import Options as FirefoxOptions
from selenium.webdriver.common.by import By
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.proxy import Proxy, ProxyType
import time
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC


proxy = Proxy({
    'socksProxy':'127.0.0.1:9150'
    })

proxy.socks_version=5

capabilities = webdriver.DesiredCapabilities.FIREFOX
proxy.add_to_capabilities(capabilities)

options = FirefoxOptions()
options.add_argument("--headless") # 不打开浏览器

s = Service('/home/dev/Desktop/geckodriver' )

driver = webdriver.Firefox( service = s, options=options, desired_capabilities=capabilities)
driver.get("https://check.torproject.org")

element = WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.XPATH, "/html/body/div[@class='content']/h1")))

# print(element)
print(element.text)


# time.sleep(2)
results = driver.find_element(By.XPATH, '/html/body/div[@class="content"]/p')
#print(results)
print(results.text)

driver.quit()
```



利用代码设置全局代理，自动上传文件。可行。但是由于文件服务器在国内，连接失败会超过max retries。

**Linux selenium访问tor的，用minio上传文件**

```Python
from minio import Minio
import urllib3

import socket
import socks
import os


if __name__ == '__main__':
    socks.set_default_proxy(socks.SOCKS5, "127.0.0.1", 9150)
    socket.socket = socks.socksocket

    filename = "1023.pdf"
    upload_file_path = "/home/dev/Desktop/upload_samples/" + filename

    # 开启tcpdump
    val = os.system('./mnt/hgfs/sharingVM/test_upload_by_tor/start_tcpdump_cmd.sh')

    # Minio只能用于HTTP代理，不能socks5代理
    minioClient = Minio(
        endpoint='49.4.114.46:9000',  # 文件服务地址
        access_key='minioadmin',  # 用户名
        secret_key='minioadmin',  # 密钥
        secure=False  # 设为True代表启用HTTPS
    )

    bucketname = "mybucket"
    buckets = minioClient.list_buckets()
    for bucket in buckets:
        print(bucket.name, bucket.creation_date)

    minioClient.fput_object(bucketname, "morning.pdf", "C1.pdf")

    # 关闭tcpdump
    val = os.system('./mnt/hgfs/sharingVM/test_upload_by_tor/stop_tcpdump_cmd.sh')
```



Selenium 官方文档，中下载可以使用库。不是核心所以不支持。直接提取到url用其他的HTTP库就可下载。



通过selenium自动上传文件用到的方法。【sendkeys()】

https://www.softwaretestinghelp.com/file-upload-in-selenium/



**Windows selenium访问tor的，模拟网页上传文件**

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

"""
@author: converse
@version: 1.0.0
@file: upload_file.py
@time: 2021/11/19 9:20

windows 版本 geckodriver
"""
from selenium.webdriver.firefox.options import Options as FirefoxOptions
from selenium.webdriver.common.by import By
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.proxy import Proxy, ProxyType
import time
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.keys import Keys

DRIVERPATH = r'D:\geckodriver\geckodriver.exe'
USERNAME = "minioadmin"
PASSWORD = "minioadmin"

options = FirefoxOptions()
# options.add_argument("--headless") # 不打开浏览器

s = Service(DRIVERPATH)

driver = webdriver.Firefox(service=s)
driver.get("http://119.3.13.158:8000/login")

element = WebDriverWait(driver, 20).until(EC.presence_of_element_located((By.XPATH, "//h1[@class='MuiTypography-root jss11 MuiTypography-h6']")))
print(element.text)

driver.find_element(By.XPATH, "//input[@id='accessKey']").send_keys(USERNAME)
driver.find_element(By.XPATH, "//input[@id='secretKey']").send_keys(PASSWORD)
driver.find_element(By.XPATH, "//button").send_keys(Keys.ENTER)
# 登录完毕
element = WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.XPATH, "//h4")))
print(element.text)

driver.get("http://119.3.13.158:8000/buckets/mybucket/browse")
upload_element = WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.XPATH, '//input[@type="file"]')))

upload_element.send_keys(r"E:\Examination\Raptor_Tor_upload_concentrate\readme.md")


if __name__ == '__main__':
    pass
```





> 当支持多文件上传的时候，可以通过 "\n"将文件路径连在一起。就可以实现多文件上传







其他4种方法：

​	https://blog.csdn.net/huilan_same/article/details/52439546