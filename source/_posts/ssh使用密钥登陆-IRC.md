---
title: ssh使用密钥登陆-IRC
date: 2022-09-06 08:33:29
tags:
---
## Step1：下载私钥
将私钥`id_rsa.irc`下载到 `~/.ssh/` (Linux或macOS) ，`C:\Users\你的用户名\.ssh` (Windows) 文件夹。如果你没有自己生成的密钥文件，可以改名为`id_rsa`，这样之后命令行或config文件登陆时就无需自行设定密钥路径
注：Linux端需要 `chmod 600 ~/.ssh/id_rsa.irc`，限定私钥权限
## Step2：设置ssh密钥登陆
### 第三方ssh软件
以MobaXterm软件为例：
![](./Moba.png)

### 命令行
```bash
ssh -p xx ywz@xxx.buaamc2.net -i "~/.ssh/id_rsa.irc" # -p参数指定端口， -i参数指定私钥路径
```
建议服务器间相互连接时使用
### config文件
编辑`~/.ssh/config` 或 `C:\Users\你的用户名\.ssh\config`，添加如下内容：
```
Host xxx
    HostName xxx.buaamc2.net
    User ywz
    Port xx
    IdentityFile "C:\Users\你的用户名\.ssh\id_rsa.irc" 
```
然后命令行输入`ssh xxx`即可
流程可以参考下图：
![](./ssh_config.png)
config文件具体使用可参考[ssh免密登陆](https://eecoder-dyf.github.io/2022/05/26/ssh-config/)

## Step3 切换到自己账号
用我们提供的私钥登陆后是ywz账号，以登陆user账号为例，可以通过命令`su user`切换到自己的账号，会提示输入账号密码

## Step4 建立自己的密钥对
假定服务器端用户为user
1. 本地命令行输入`ssh-keygen`，根据提示依次输入密钥路径名称（如果之前没有`~/.ssh/id_rsa`文件，推荐默认，这样就不用指定特定密钥）、密码（可以没有）。得到私钥`id_rsa`和公钥`id_rsa.pub`
2. 在服务器禁止密码登录的情况下，无法使用`ssh-copy-id`的方法将公钥添加到服务器的`~/.ssh/authorized_keys`，需要复制本地的公钥`~/.ssh/id_rsa.pub`或`C:\Users\你的用户名\.ssh\id_rsa.pub`中的内容至服务器端的`/home/user/.ssh/authorized_keys`，追加在最后一行（如果没有，就新建一个文件）

然后就可以使用自己的密钥登陆自己的账号了，此外要**注意权限管理**，不然可能会出现拒绝访问的现象
```bash
chmod 750 /home/user/ #也可以刷700
chmod -R 600 /home/user/.ssh/
```
其他：[ssh私钥修改密码](https://www.jianshu.com/p/b1bdaea5e5c8?u_atoken=f6db5758-f46b-4ee1-a7ef-a1376fddf858&u_asession=01PyZhzoP4HzaUL5X83JU6XOUkLSsETWUJh1pn0uM5ELTgrRtzlODL-tZo-NEvqkOxX0KNBwm7Lovlpxjd_P_q4JsKWYrT3W_NKPr8w6oU7K97Z3au9yv4BfD63ElUK8w-zdjoMV1y19BFQvaXcOyBfmBkFo3NEHBv0PZUm6pbxQU&u_asig=05mY10hmEtdZbUAenCqysz0YcBOKCrOu66d7HbC3zmXSSG7-PFS8Torr8_T1VMVN60k7tEWhn6T6UftUGbMBVMEaRPB1IxhlwtumIm4_X0g45ZUXUa6nloj0d81JyZqC7o_GE-Jh0H_kt119YJH4onQDiXmFkwLRZns_XG-fWNnsz9JS7q8ZD7Xtz2Ly-b0kmuyAKRFSVJkkdwVUnyHAIJzZKOHwVLayKDdAJnbYr3LUHj3_EqHwobBSX6lLzyE2J0qBR97QLsOYcZJeUxi-_JXu3h9VXwMyh6PgyDIVSG1W8gzvE26WgQmdNtVY9p_a43MqIUE1FeE2xjUDMIFCPoAcPm0_70rY2940tUHVQ8_WZrRRzSXG8uk_FhwUwRIpwumWspDxyAEEo4kbsryBKb9Q&u_aref=OeSsskrZmNmE3GFlmSBzsQkXZDE%3D)