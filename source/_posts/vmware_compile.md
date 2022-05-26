---
title: 更新Linux内核后重新打开vmware报错
---

每次Linux更新内核后，需要重新编译vmware，可参考以下命令：

```shell
# /bin/bash
# git clone https://github.com/mkubecek/vmware-host/modules.git
# cd vmware-host-moudles
git pull
git checkout workstation-16.2.3
sudo make
sudo make install
sudo /etc/init.d/vmware restart
```