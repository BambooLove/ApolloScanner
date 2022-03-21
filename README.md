# ApolloScanner
自动化巡航扫描框架（可用于红队打点评估）
![图片](https://user-images.githubusercontent.com/11972644/158723361-8356e64d-55fa-40df-a39c-2b52561726ab.png)


## 安装
+ python版本： 3.8.x 或 3.9.x
+ django版本：4.0.1
+ nmap：需要
+ masscan：需要
+ mysql
+ 前端：基于simple-ui
+ 支持操作系统：MacOS Monterey 12.3 / Ubuntu 18.04 LTS

```python
sudo python3 -m pip install -r requirments.txt
sudo python3 manage.py migrate
sudo python3 manage.py createsuperuser
sudo python3 manage.py runserver
```

## 功能
+ 资产收集（需要主域名，资产对象可直接在爆破和漏扫过程中调用）
  + 子域名收集（需要virustotal-api-token）
  + cname收集
  + ip地址（a记录）收集
  + 开放端口扫描（基于masscan）
  + 端口对应服务、组件指纹版本探测（基于nmap）
  + http标题探测
  + http框架组件探测

+ github敏感信息收集
  + 基于域名和关键字的敏感信息收集（需要github-token）
  + 
+ 暴力破解（基于exp的暴力破解）
  + exp注册模块
    + 代码动态编辑
    + 代码动态调试
    + 支持资产对象
  + 破解任务模块
    + 支持exp对象调用
    + 支持资产对象
    + 支持批量资产 
    + 支持多线程（可配置） 
  + 破解结果模块
    + 支持结果显示
    + 支持钉钉通知
  + 敏感路径探测任务
  + 敏感路径探测结果  
  
+ 漏洞扫描模块
  + exp注册模块 
    + 代码动态编辑
    + 代码动态调试
    + 支持资产对象
  + 漏扫任务模块
    + 支持exp对象调用
    + 支持资产对象
    + 支持批量资产 
    + 支持多线程（可配置) 
  + 结果显示模块
    + 支持结果显示
    + 支持钉钉通知
   
+ 配置模块
  + 支持常用系统配置（各类token、线程数）
  + 支持用户、用户组、权限配置模块
  + 支持启动服务模块
    + HTTP服务（支持HTTP请求记录）
    + DNS服务（支持DNS请求记录） 

## exp编写规范
+ 暴力破解
```python
def brute_scan_function_name(ipaddress, port, username, password, logger):  
    import xx_module # 引入模块全部在函数内容写
    # ... 
    # ...是爆破exp核心代码
    logger.log("xxxxx") # 代替print
    return True  # 返回必须是true/false
```
+ 漏扫扫描
```python
def brute_scan_function_name(ipaddress, port, logger):  
    import xx_module # 引入模块全部在函数内容写
    # ... 
    # ...是漏扫exp核心代码
    logger.log("xxxxx") # 代替print
    return True  # 返回必须是true/false
```

## 报错解答
### 1、缺乏mysql_config命令：
+ 报错示例
```
Preparing metadata (setup.py) ... error
error: subprocess-exited-with-error

× python setup.py egg_info did not run successfully.
│ exit code: 1
╰─> [11 lines of output]
/bin/sh: 1: mysql_config: not found
Traceback (most recent call last):
File "", line 2, in
File "", line 34, in
File "/tmp/pip-install-2er683ou/mysqlclient_5ba8560cf6ca429b8316cf1cf6771c9a/setup.py", line 16, in
metadata, options = get_config()
File "/tmp/pip-install-2er683ou/mysqlclient_5ba8560cf6ca429b8316cf1cf6771c9a/setup_posix.py", line 51, in get_config
libs = mysql_config("libs")
File "/tmp/pip-install-2er683ou/mysqlclient_5ba8560cf6ca429b8316cf1cf6771c9a/setup_posix.py", line 29, in mysql_config
raise EnvironmentError("%s not found" % (_mysql_config_path,))
OSError: mysql_config not found
[end of output]

note: This error originates from a subprocess, and is likely not a problem with pip.
error: metadata-generation-failed

× Encountered error while generating package metadata.
╰─> See above for output.
```
+ 解析：由于部分环境缺乏mysql_config命令导致mysqlclient依赖安装失败，可能是由于没有安装该命令或者没有建立该命令的软连接，可根据自己环境google解决。
+ 参看文献 : ![解决Mysql中mysql_config not found的方法](https://www.cnblogs.com/alice-bj/articles/9512426.html)
