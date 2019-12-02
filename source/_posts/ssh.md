---
title: ssh连接局域网内其他主机
date: 2018-10-27 19:30:24
tags: ssh
categories: 后端
description:
---
## 前言
虽然连接内网其他主机的意义不大，但是却能验证一些问题。

例如前一篇[ssh连接内网主机A](https://bigcaol.github.io/2018/10/26/ssh%E8%BF%9E%E6%8E%A5%E5%86%85%E7%BD%91%E4%B8%BB%E6%9C%BAA/)文章中，所有设置都没问题却发现最后每次连接内网主机A时都会被拒绝。于是尝试局域网内连接主机A同样被拒绝，查阅资料发现原来主机A的sshserver没有安装。（ubuntu默认安装了ssh-client，但没有安装sshserver)(**sudo apt-get install openssh-server**解决！)
## 想要达到的效果
局域网内使其他主机能够通过SSH连接主机A
## 配置主机A开启ssh-server
### 检测是否安装了ssh-server
```
ps -ef|grep sshd
```
如果有像下面sshd字样一般是安装了server，如果不放心可以重新装一遍。
![sshd](https://i.loli.net/2018/10/27/5bd454c49efa7.png)
```
sudo apt-get install openssh-server   #安装ssh-server
```
重启ssh-server
```
sudo /etc/init.d/ssh stop
sudo /etc/init.d/ssh start
```
### 确定端口号
一般默认端口是22，但是确认一下比较好
```
 cat /etc/ssh/ssh_config   #
```
![port](https://i.loli.net/2018/10/27/5bd45754ba6f6.png)
可以看到没有设置端口号（因为#把port注释了），可以自己定义成其他端口号，如222。修改后需要重启ssh-server。
### 确定主机A的ip地址
```
ifconfig
```
![ip](https://i.loli.net/2018/10/27/5bd4593a43af8.png)
## 其他主机连接主机A
现在在其他主机上执行下面命令：
```
ssh -p 22 userA@192.168.1.46   
#userA是主机A的一个用户
#接下来需要输入主机A的密码即可
```
完～