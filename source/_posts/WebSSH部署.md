---
title: WebSSH部署
date: 2024-01-17 20:03:43
tags:
---
# 部署
参考如下[仓库](https://github.com/Jrohy/webssh)
直接使用docker进行部署
```bash
docker run -d -p 5032:5032 --log-driver json-file --log-opt max-file=1 --log-opt max-size=100m --restart always --name webssh -e TZ=Asia/Shanghai -e savePass=true jrohy/webssh
```
# 使用
访问 http://hostip:5032 即可，其中hostip即为部署docker的服务器ip

在北航校外可以访问 https://d.buaa.edu.cn
然后在最上方的访问栏选择http并输入`hostip:5032`即可访问校内SSH资源，并且可以借此**传输大文件**，传输带宽能达到20MB/s, 远强于跳板机