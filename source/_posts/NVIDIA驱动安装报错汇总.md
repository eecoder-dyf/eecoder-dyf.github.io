---
title: NVIDIA报错汇总
date: 2024-03-12 10:51:32
tags:
---
### An error occurred while performing the step: “Building kernel modules”
一般是gcc版本不对，最新显卡驱动（2024-03-12）需要的gcc版本是gcc-12而Ubuntu 22.04默认是gcc-11，需要手动安装gcc-12
```bash
sudo apt install gcc-12
rm /usr/bin/gcc
ln -s /usr/bin/gcc-12 /usr/bin/gcc
```
### RuntimeError: CUDA unknown error - this may be due to an incorrectly set up environment, e.g. changing env variable CUDA_VISIBLE_DEVICES after program start. Setting the available devices to be zero.
具体表现为torch找不到某个device，解决方式：
```bash
sudo modprobe -r nvidia_uvm && sudo modprobe nvidia_uvm
```