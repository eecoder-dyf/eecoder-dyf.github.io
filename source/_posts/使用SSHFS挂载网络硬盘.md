---
title: 使用SSHFS挂载网络硬盘
date: 2023-05-18 10:16:47
tags:
---
## 简介
SSHFS（SSH Filesystem）是一个基于FUSE的文件系统客户端，用于通过SSH连接远程目录。SSHFS使用的是SFTP协议，它是SSH的一个子系统，在大多数SSH服务器上默认启用

与其他网络文件系统（如NFS和Samba）相比，SSHFS的优势在于它不需要在服务器端进行任何额外的配置。要使用SSHFS，您只需要具有SSH访问远程服务器的权限。

使用网络文件分享，主要优势是在网速够快的情况下可以多台服务器共用一份数据，省去不同服务器之间传输的麻烦，也节省硬盘空间

## 安装
### 服务器端
无需任何额外配置，只需要安装好OpenSSH服务器即可

**Ubuntu**：
```bash
sudo apt install openssh-server # 系统管理员配置，普通用户无需配置
```

**Windows 10/11：** \
Step1: 设置-应用-可选功能-添加可选功能-搜索“OpenSSH服务器”并安装 \
Step2: Win+R 打开运行窗口，输入services.msc 打开服务，找到"OpenSSH SSH Server"一项，打开并修改**启动类型**为“自动”

### 客户端
**Ubuntu**：
```bash
sudo apt install sshfs # 系统管理员配置，普通用户无需配置
```
**Windows** \
安装三个软件：
* sshfs-win：https://github.com/billziss-gh/sshfs-win/releases
* winfsp：https://github.com/billziss-gh/winfsp/releases
* SSHFS-Win Manager：https://github.com/evsar3/sshfs-win-manager/releases (GUI，可选)

## 使用
**Ubuntu**：\
连接方法与scp的用法类似假设远程服务器为 usr@remoteip 端口为port， 要挂载的路径为/home/usr/share， 本地装载路径为/mnt/remote1(需要为空文件夹)
```bash
sshfs -p port usr@remoteip:/home/usr/share /mnt/remote1
```
注：如果Windows作为服务器，文件分享给Linux，其ssh的路径输入方法为把反斜杠改成但正斜杠，如果有空格或中文，可以给路径加上引号，如：
```bash
sshfs usr@remoteip:"C:/Users/user/远程 分享" /mnt/remote1 #scp同理
```
此外也可以直接利用OpenSSH-config文件，使用代号以及免密登录，参见[博客](https://eecoder-dyf.github.io/2022/05/26/ssh-config/) \

取消连接：
```bash
fusermount -u /mnt/remote1
```
**Windows**: \
建议直接使用GUI界面 打开SSHFS-Win Manager 配置SSH连接即可，参见[博客](https://blog.csdn.net/xieqiaokang/article/details/109557482)

