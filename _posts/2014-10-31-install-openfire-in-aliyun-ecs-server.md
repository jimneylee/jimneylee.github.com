---
author: jimneylee
layout: post
title: "阿里云ECS服务器配置ubuntu安装openfire服务器"
comments: true
tags:
- openfire
- xmpp
---

最近搞了一台[阿里云的ECS服务器](http://buy.aliyun.com/?spm=5176.383338.5.4.RsiCqv)，因为搞活动半年免费，所以就申请了一台，过两天就批准下来。因为固定IP，所以多花了1百多RMB买了固定IP。总体说了还是挺值得，觉得一个用浪费，分享出来跟大家一起玩玩。

搞台服务器主要为了学习即时聊天功能模块的开发，服务器采用[OpenFire](http://www.igniterealtime.com/)，i
OS前端基于[XMPPFrameWork](https://github.com/robbiehanson/XMPPFramework)，经过一段时间学习，写了一个[开源作品JLWeChat](https://github.com/jimneylee/JLWeChat-iPhone)，已从oschina的私有仓库转移到github，欢迎参与讨论交流。

主要功能类似微信的简单聊天功能，包括表情、图片、音频，后台存储基于[七牛](http://www.qiniu.com/)免费提供云存储。

下面记录下在阿里云ECS服务器配置OpenFire的过程，需要的同学可以参考下，少走弯路。

#### 下载OpenFire安装文件

1、安装Axel

[Axel是一个命令行下载工具](http://wiki.ubuntu.org.cn/Axel)

	$ apt-get install axel

2、下载OpenFire安装文件，目前最新为3.9.3

	$ wget -O openfire.tar.gz http://www.igniterealtime.org/downloadServlet?filename=openfire/openfire_3_9_3.tar.gz

3、解压到/opt
> If using the .tar.gz, extract the archive to /opt or /usr/bin:

	$ tar -xzvf openfire_3_9_3.tar.gz
	$ mv openfire /opt

### 安装mysql
1、执行安装命令

	$ netstat -tap | grep mysql

如果遇到下面这个错误，请执行下面操作
> apt-get install mysql-server : Depends: mysql-server-5.5 but it is not going to be installed

	$ apt-get autoremove mysql* --purge
	$ apt-get remove apparmor
	$ apt-get install mysql-server mysql-common

### 创建OpenFire需要的数据库
	
	$ mysql -u

	mysql> create database openfire;
	mysql> use openfire
	mysql> source /opt/openfire/resources/database/
	openfire_mysql.sql;

### 启动OpenFire服务器
	$ /opt/openfire/bin/openfire start

### 网页配置OpenFire
1、数据库设置如下
	
jdbc:mysql://121.41.129.248:3306/openfire?useUnicode=true&characterEncoding=UTF-8&characterSetResults=UTF-8