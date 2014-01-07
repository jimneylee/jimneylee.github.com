---
author: jimneylee
layout: post
title: "MacOS快速安装OpenCV"
date: 2012-10-10 09:10:11
comments: true
tags:
- MacOS
- OpenCV
- port
---

最近想学习OpenCV，在我的MacMini上安装这个很不错的开源图像处理库。由于安装编译OpenCV，需要cmake来编译。cmake又需要make工具和gcc编译器。一连串需要安装下载的东西很多。

摸索了半天发现一条便捷的快速搭建OpenCV学习环境的方法。记录在此，日后待用。有需要的同学参考下。

当前安装环境：`MacMini、Mac OS X Lion10.7.3、XCode4.3`

下载安装如下几个工具：

1、XCode安装`Command Line Tools`文件，包含所需的gcc和make工具，或用开发者账号登录[官方下载](https://developer.apple.com/downloads/index.action)。

2、下载安装适合当前系统的[MacPorts](https://distfiles.macports.org/MacPorts/)文件，类似linux下的`apt-get`命令。`port`命令[使用手册](http://guide.macports.org/#installing.macports)

3、上面安装完成后，打开`Terminal`终端，运行如下：

{% highlight bash %}
$ sudo port selfupdate // 更新自身到最新版本
$ sudo port list opencv // 查看要下载的opencv的版本号，此步骤可忽略
$ sudo port install opencv // 等待安装，此步骤时间较长
{% endhighlight %}

安装路径默认位于`/opt/local/include`和`/opt/local/lib`
建立一个command line工程，进行简单测试。

1、设置工程库文件和头文件引入目录，如下图
![](/img/posts/archives/quick-install-opencv-in-macos/set-search-paths.jpg)

2、引入两个动态链接库，位于/opt/local/lib
![](/img/posts/archives/quick-install-opencv-in-macos/add-two-dynamic-lib.jpg)

3、编写最简单的测试代码，资源放在工程目录下
![](/img/posts/archives/quick-install-opencv-in-macos/sample-code.jpg)

4、运行结果如下图，分别为isColor=1(原图显示)和isColor=0(黑白显示)
![](/img/posts/archives/quick-install-opencv-in-macos/result-color-image.jpg)
![](/img/posts/archives/quick-install-opencv-in-macos/result-blackwhite-image.jpg)