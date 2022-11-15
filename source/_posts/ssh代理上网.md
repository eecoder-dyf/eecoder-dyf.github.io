---
title: ssh代理上网
date: 2022-11-15 16:45:26
tags:
---
## 通过ssh -D命令建立socks5代理上网
本方法适用于机器可以连接内网，无法连接外网，但内网中有其他机器可以上外网的情况
### step1: 建立ssh socks5代理
在上不了网的内网机器上输入如下命令，以1080端口为例，若已经被占用，可自行修改
```bash
ssh -ND localhost:1080 usr@serverip -p 22
```
参数解释：`-N`为后台运行， `-D`为建立socks5代理的关键命令 `localhost:1080`是本机以及需要监听代理的端口，`usr@serverip -p 22`即为能上网的机器及其ssh服务的端口，建议使用ip登录不要用域名，因为可能访问不了DNS服务器
### step2：使用socks5代理上网
**1. python使用socks5**
首先在pypi上下载[pysocks](https://files.pythonhosted.org/packages/8d/59/b4572118e098ac8e46e399a1dd0f2d85403ce8bbaad9ec79373ed6badaf9/PySocks-1.7.1-py3-none-any.whl)包，然后用pip进行安装
**2. 使用socks5代理上网**
以pip安装为例，安装pysocks后可以用--proxy参数进行代理上网
```bash
pip install numpy --proxy="socks5://127.0.0.1:1080"
```
命令行整体代理：
```bash
export ALL_PROXY=socks5://127.0.0.1:1080
```
有关更多其他应用使用socks5代理的方法，请自行搜索。部分命令不支持使用export命令走命令行全局代理，需要自行配置
温馨提示：wget使用socks5代理较为复杂，建议使用curl命令下载文件

**PS. MC-2实验室2080服务器无法上网原因**
本质原因是无线网卡被错误启动，需要禁用掉无线网卡，但无线网卡会自动重启，在服务器重启前没有找到可行方案，可以通过如下方式解决
![](./2080.png)
*1. 无法登录*：解决方案是[配置跳板机](https://github.com/ywz978020607/History/tree/master/cv%E7%A0%94%E7%A9%B6%E7%94%9F%E6%97%A5%E5%B8%B8Lab/%E7%8E%A9%E8%BD%AC%E8%B7%B3%E6%9D%BF%E6%9C%BA)，使用207机房的其他机器如30901，30902，207，401跳转登录
*2. 无法上网*：先切换管理员账号禁用掉无线网卡`sudo ifconfig wlp5s0 down`，然后按照上面的步骤配置socks5代理，若发现网卡重新唤醒，继续禁用

