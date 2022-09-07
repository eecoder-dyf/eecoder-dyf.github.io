---
title: Ubuntu下拔除硬盘的正确方式
date: 2022-06-15 09:15:05
tags:
---
## Linux下通用移动硬盘移除方法
在Windows下，我们拔除移动硬盘时一般是在右下角弹出硬盘移动设备。 
但是在Linux，尤其是命令行时该如何操作呢？

以硬盘`/dev/sda`为例
### 第一步：取消硬盘挂载
```bash
udisksctl unmount -b /dev/sda1
```
这里用到了udisksctl这个工具，不用这个工具也可以，直接`sudo umount -l /dev/sda1`
### 第二步：断开电源
```bash
udisksctl power-off -b /dev/sda
```
这样才算真正弹出了硬盘，可以安心拔出了
另外u盘一般可以不断电直接拔除，最多会造成正在读写的数据丢失，不会对磁盘硬件造成影响