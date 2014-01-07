---
author: jimneylee
layout: post
title: "iOS直接连接MacOS的MySQL服务器"
date: 2012-02-12 08:52
comments: true
tags:
- mysql
- xcode
- macos
- iphone
---

**一、在MacOS上搭建MySQL Server,本文采用Mac10.6.x，支持64位**

１、根据系统版本和支持位数，[下载MySQL Server的dmg文件](http://dev.mysql.com/doc/refman/5.1/en/macosx-installation.html)

２、dmg包里面有三个文件

* a、安装`mysql-5.1.61-osx10.6-x86_64.pkg`

* b、安装`MYSQL.prefPane`,可以在系统偏好设置（system preference）的其他（Other）栏显示。可以启动和停止Server。

* c、（可选）安装`MySQLStartupItem.pkg`支持开机启动

３、将安装目录下`support-files`目录中的`my-small.cnf`拷贝到ect中，并更名为`my.cnf`，MySQL启动时加载的配置文件

{% highlight bash %}
$ cd /usr/local/mysql-5.1.61-osx10.6-x86_64
$ sudo cp support-files/my-small.cnf /etc/
$ mv /ect/my-small.cnf /ect/my.cnf
{% endhighlight %}
４、初始root密码为空，所以需要设置root密码

{% highlight bash %}
$ mysqladmin -u root -p password [password]
{% endhighlight %}

5、登录测试，在系统设置偏好（system reference）中MySQL开启运行Server，并做些sql指令操作（本文省去）

{% highlight bash %}
$ mysql -h localhost -u root -p
{% endhighlight %}

6、关于字节编码需要做一些简单配置修改,初始默认支持`latin`，所以数据库存入中文`utf８`后，查询显示为乱码

* a、显示字符集编码

{% highlight bash %}
$ mysql> show variables like'character%';
{% endhighlight %}

* b、修改配置文件，完成后在登录mysql，运行上面的a操作，看字节编码是否已经全部为utf８，参考此处sql编码

{% highlight bash %}
$ sudo vi /ect/my.cnf
{% endhighlight %}
>[client]

>default-character-set = utf8

>[mysqld]

>default-storage-engine = INNODB

>character-set-server = utf8

>collation-server = utf8_general_ci

>default-character-set = utf8

至此，完成了MySQL　Server的安装与测试。过程跟linux基本一样，都是类unix系统。

**二、iOS系统上访问MySQL Server**

在iPhone/iPad上访问MySQL Server，是通过官方MySQL Client库来实现。iOS完全兼容C语言，所以通过官方的Client源码生成静态库来调用。
我们将制作这个静态库`libmysqlclient.a`，支持Simulator和iOS设备。

１、下载client的源码，当前版本(mysql-connector-c-6.0.2.tar.gz)

２、源码采用cmake进行编译，所以请下载最新的cmake编译工具

３、通过终端进入到`mysql-connector-c-6.0.2`目录下，运行下面命令，生成XCode工程

{% highlight bash %}
$ cmake -G Xcode
{% endhighlight %}
在这里推荐一个MacOS开源插件[cdto](https://github.com/jbtule/cdto)。类似ubuntu系统下，在可视化的目录窗口，终端进入直接对应的目录路径，挺方便的。

４、修改一些文件，不然编译会出错

* a、在my_net.h文件中，注销掉47和49行

{% highlight c %}
//#include <netinet/in_systm.h>
#include <netinet/in.h>
//#include <netinet/ip.h>
{% endhighlight %}

* b、在my_global.h，129行编译宏添加arm编译选择

{% highlight c %}
#if defined(__i386__) || defined(__ppc__) || defined(__arm__)
{% endhighlight %}

* c、在my_config.h，注销掉６３行

５、通过`xcodebuild`命令为simulator和ios设备，生成对应的编译库，然后通过`lipo`生成一个统一包。首先在终端（terminal）cd到源码的`libmysql`路径下，运行如下命令：

* a、生成simulator静态包，注意sdk选择与mac系统中一致

{% highlight bash %}
$ xcodebuild -project ../libmysql.xcodeproj -target mysqlclient -configuration Release -sdk iphonesimulator4.3 ONLY_ACTIVE_ARCH=NO ARCHS=i386 PRODUCT_NAME=mysqlclient_simulator
{% endhighlight %}
* b、生成ios设备静态包

{% highlight bash %}
$ xcodebuild -project ../libmysql.xcodeproj -target mysqlclient -configuration Release -sdk iphoneos4.3 ONLY_ACTIVE_ARCH=NO ARCHS="armv6 armv7" PRODUCT_NAME=mysqlclient_device
{% endhighlight %}

* c、使用lipo打在一起，保存在Release-iphoneos路径下

{% highlight bash %}
$ lipo ./Release-iphonesimulator/libmysqlclient_simulator.a ./Release-iphoneos/libmysqlclient_device.a -create -output ./Release-iphoneos/libmysqlclient.a
{% endhighlight %}
至此所用的静态包已经打好，库头文件为源码目录中的include目录下所有的头文件。

**注意**：今天测试发现，在lion系统下打的SDK 5.0的`libmysqlclient.a`到Snow Leaporad编译出错，无法使用，所以在Snow Leaporad下重新编译这个库，可以正常使用，可能跟上面编译选项有关，如果有路过的兄弟知道原因，请不吝赐教。

三、访问测试，使用库测试参考MYSQL C API

通过XCode建立一个视图工程，在视图viewDidLoad中添加如下代码：

{% highlight c %}
#import "mysql.h"
MYSQL *mysql = mysql_init(nil);
if(mysql_real_connect(mysql, localhost, username, passwd, dbname, 3306, NULL, 0)) {
    //set encode utf８
    mysql_set_character_set(mysql, "utf8");
    NSLog(@"Connection Successful!");
    //TODO:CRUD
}
else {
    NSLog(@"Connection Failed");
}
{% endhighlight %}
**后记**

今天发现这些东西梳理出来写成博文的过程，跟code有一样的体验。理清思路，把知道的东西简洁表达而不被误解，还是要有一些思考和推敲。好长时间没写东西，纲领概要啥都无知，今后也要多写写。软件开发忌讳重复劳动，这意味额外的代价。人生短暂，伤不起也损不起。如果这篇博文能为您节省生命，乃我大幸大乐也！
