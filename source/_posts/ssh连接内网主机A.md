---
title: ssh连接内网主机A
date: 2018-10-26 19:02:18
categories: 后端
tags:
    -SSH
    -后端
---
![基本](https://i.loli.net/2018/10/26/5bd30be7819df.png)
## 假设基本信息
假设外网主机 B 的 ip 是 **110.73.180.212**，拥有一个名为 **userB** 的用户，ssh 对外暴露的端口号是 **4086**
内网主机A：我们想要连接的主机，**userA**，SSH端口**22**
## 内网A免密码连接公网B
```bash
ssh-keygen   #生成密钥
```
```bash
cat .ssh/id_rsa.pub  #查看是否生成了密钥
```
```bash
ssh-copy-id -i .ssh/id_rsa.pub uaerB@110.73.180.212 -p portB  
#使用自己的B的用户名和主机ip，-p后是公网B的SSH连接端口，默认一般是22，不过搬瓦工的一般是随机的，需要自己查看。
#需要输入密码
```
```bash
ssh -p '4086' 'userB@110.73.180.212'
#应该就可以免密码连接主机B了
```
## 修改外网主机B允许ssh转发
```shell
vim /etc/ssh/sshd_config   #添加 GatewayPorts yes
GatewayPorts yes
```
```bash
service sshd restart   #重启SSH
```
## 在内网A上配置反向隧道
基本方法：
```bash
$ ssh -N -f -R 1111:localhost:22 userB@110.73.180.212 -p 4086
# -N 参数表明我们只做端口转发，而不执行远程命令。
# -R 1111:localhost:22 则表明当有人试图用 1111 端口来连接主机 B 的时候，就把它转发给主机 A 的 22 端口（即主机 A 的本地 ssh 端口）。1111可以更改为不冲突的任意端口
# -f 参数，只是指定 ssh 在后台运行而已。
# -p
```
优化方法：
```bash
autossh -M 1122 -N -f -R 1111:localhost:22 userB@110.73.180.212 -p 4086
# 仅多了一个-M参数，这个参数的意思就是用本机的1122端口来监听ssh，每当他断了就重新把他连起来
# 若autossh未安装，先安装
sudo apt-get install autossh
```
利用一个额外的端口监控1111端口，每当1111断了就重新把他连起来
![优化](https://i.loli.net/2018/10/26/5bd30be790897.png)
## 最后在其他机器上连接内网A
```bash
ssh userA@110.73.180.212 -p 1111 #登录内网机器A
# userA是主机A上的一个用户
```
***
## 参考
-[外网主机通过 ssh 连接局域网主机](https://www.synscope.com/1066/外网主机通过ssh连接局域网主机/)
-[利用反向ssh从外网访问内网主机](https://blog.mythsman.com/2017/01/14/1/)
-[SSH内网穿透的N种姿势](https://blog.csdn.net/MasonQAQ/article/details/78190400)





