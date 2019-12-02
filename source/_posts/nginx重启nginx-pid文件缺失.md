---
title: nginx重启nginx.pid文件缺失
date: 2018-11-04 18:04:57
tags:
categories:
---
{% cq %}## 前言 {% endcq %}
最近重启服务器后发现nginx不能工作，网上查找原因解释说“关闭nginx时，把其nginx.pid会被删掉”
{% cq %}## 解决过程 {% endcq %}
查看nginx进程
```bash
ps -ef | grep nginx
```
<!-- more -->
不存在master进程，只存在php-fpm进程。
![不存在master进程](https://upload-images.jianshu.io/upload_images/14769852-477923ad678ac1dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
重新加载出错，找不到nginx.pip文件
```bash
sudo /opt/nginx-1.7.8/sbin/nginx -s reload    #重新加载nginx
nginx: [error] open() "/opt/nginx-1.7.8/logs/nginx.pid" failed (2: No such file or directory)
```
从conf文件平滑启动，但又出现问题，说找不到access.log文件。我的access.log文件是存在的，只不过不在```/var/log/nginx/```下，复制一份过去。

```bash
sudo /opt/nginx-1.7.8/sbin/nginx -c /opt/nginx-1.7.8/conf/nginx.conf    #nginx.conf是我的配置文件
nginx: [emerg] open() "/var/log/nginx/access.log" failed (2: No such file or directory)
```
复制一份过去后，重新执行
```bash
sudo /opt/nginx-1.7.8/sbin/nginx -c /opt/nginx-1.7.8/conf/nginx.conf    #nginx.conf是我的配置文件
```
没有报错，再看一下进程：
```bash
ps -ef | grep nginx
```
![master进程工作](https://upload-images.jianshu.io/upload_images/14769852-a3c5cac114077674.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
{% cq %} ## 完～ {% endcq %}


    