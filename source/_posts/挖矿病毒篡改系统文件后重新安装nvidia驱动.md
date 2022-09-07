---
title: 挖矿病毒篡改nvidia驱动文件后重新安装nvidia驱动
date: 2022-09-07 20:01:36
tags:
---
最近百航校内挖矿病毒猖獗，具体表现为篡改/usr/bin/nvidia-smi文件，使得nvidia-smi命令打印输出表现为挖矿前状态，但该命令其他参数均不可用
通过下述方法，可以快速重装驱动
注：以下命令需要root或sudo下执行
### Step 1 停止所有显卡进程
```bash
systemctl isolate multi-user.target
modprobe -r nvidia-drm
```
否则安装驱动时可能会报错：
An NVIDIA kernel module 'nvidia-drm' appears to already be loaded in your kernel. This may be because it is in use (for example, by an X server, a CUDA program, or the NVIDIA Persistence Daemon), but this may also happen if your kernel was configured without support for module unloading. Please be sure to exit any programs that may be using the GPU(s) before attempting to upgrade your driver. If no GPU-based programs are running, you know that your kernel supports module unloading, and you still receive this message, then an error may have occured that has corrupted an NVIDIA kernel module's usage count, for which the simplest remedy is to reboot your computer.

### Step2 接触`/usr/bin/nvidia-smi`的保护
在安装驱动过程中，可能会出现'cannot delete /usr/bin/nvidia-smi'的报错，这是因为病毒不但篡改了`/usr/bin/nvidia-smi`而且给它写上了保护。
可以通过lsattr命令查看文件属性
```bash
lsattr /usr/bin/nvidia-smi
----i---------e------- /usr/bin/nvidia-smi #输出
```
其中i表示不得任意更动文件或目录
可以通过chattr命令解除保护:
```bash
chattr -i /usr/bin/nvidia-smi
```
### Step3 下载nvidia驱动并安装
[下载网址](https://www.nvidia.cn/Download/index.aspx?lang=cn)
选择对应显卡（其实Linux端Geforce显卡驱动都一样，选不选无所谓）及Linux-64bit即可，下载下来是.run文件，直接执行
```bash
sh NVIDIA-Linux-x86_64-515.48.07.run
```
然后按照提示安装即可，选项可以都选yes