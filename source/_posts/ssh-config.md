---
title: ssh配置文件
date: 2022-05-26 11:35:26
tags:
---

# ssh配置免密登录及vscode

首先执行: `ssh-keygen -t rsa`， 生成密钥文件  

可以看到 `~/.ssh`目录下生成了两个文件id_rsa，id_rsa.pub分别是私钥和公钥 

然后把公钥复制给远程主机，需要远程主机ssh配置了允许密钥登录才能生效

```shell
ssh-copy-id -i ~/.ssh/id_rsa.pub usrname@hostip #-i指定公钥，用默认公钥时可以不加
```

或

```shell
ssh-copy-id -i ~/.ssh/id_rsa.pub Host
```

例如如下的两种形式


copy过程需要密码， 可以在远程主机的`~/.ssh/authorized_keys`中看到新添加的公钥

后一种需要先配置config文件在`~/.ssh`文件夹下  

参考配置如下：

```
Host anyname
  HostName 10.xxx.xxx.xxx
  User nnn
  IdentityFile "~/.ssh/id_rsa"
```

此时可以直接`ssh Host`来登录, `scp Host:dir`来代替原scp命令, 其他命令如rsync等也能替代, 例如:

```shell
ssh nnn@10.xxx.xxx.xxx -> ssh anyname
scp ./file nnn@10.xxx.xxx.xxx:~/sharedir -> scp ./file anyname:~/sharedir
```

Windows平台同样适用，不过需要先安装OpenSSH客户端、服务器：*设置-应用-可选功能-添加可选功能*（如果没有，在*已安装功能*中看是否已经安装）。如果IdentityFile的相对路径报错就用绝对路径（注意Windows平台反斜杠），例如

```
IdentityFile "C:\Users\dyf\.ssh\id_rsa" 
```

该配置也可用于VScode的Remote SSH，写好就能直接看到

### 可能遇到的其他问题：

若之前连接过主机，但主机发生了变更(例如重装系统，换机器等)，但ip没变，可能会报错，此时需要执行：  

```shell
ssh-keygen -R "[cn.buaamc2.net]:22" #[ip or domain]:port
```

把该ip主机从known_hosts中删除

Windows机器无法使用ssh-copy-id命令，在使用前先执行如下内容：
```powershell
function ssh-copy-id([string]$userAtMachine, $args){   
    $publicKey = "$ENV:USERPROFILE" + "/.ssh/id_rsa.pub"
    if (!(Test-Path "$publicKey")){
        Write-Error "ERROR: failed to open ID file '$publicKey': No such file"            
    }
    else {
        & cat "$publicKey" | ssh $args $userAtMachine "umask 077; test -d .ssh || mkdir .ssh ; cat >> .ssh/authorized_keys || exit 1"      
    }
}
```