---
title: Linux创建新用户
date: 2022-10-15 16:37:16
tags:
---
```bash
useradd -d /home/yourname -m yourname #添加用户，指定家目录
passwd yourname	#设置密码
usermod -s /bin/bash yourname #设置bash为默认shell
```

**若需要sudo权限**
```bash
usermod -a -G sudo yourname	#添加用户组到sudo
usermod -a -G adm yourname	#添加用户组到admin
id yourname	#查看用户信息(id,用户组等)
```

**若硬盘空间不够，可参考以下移动用户文件夹**
```bash
mv /home/yourname /media/data/ #迁移用户文件夹
ln -s /media/data/yourname /home/yourname	#创建软链接
chown -R yourname:yourname /media/data/yourname	#授予访问目录权限
chmod 760 /media/data/yourname	#给该目录设权限
```