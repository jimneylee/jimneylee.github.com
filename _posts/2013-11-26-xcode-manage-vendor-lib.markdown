---
author: jimneylee
layout: post
title: "iPhone开发如何管理第三方依赖库"
date: 2013-11-26 11:44:00
comments: true
tags:
- git
- submodule
- cocoapods
---

采用`git submodule`和`CocoaPods`结合管理，如果依赖的库支持`CocoaPads`，优先采用`CocoaPads`，类似Ruby环境的gem工具。如果被依赖库不支持，则采用`git submodule`。通过依赖管理第三方库，主要为了日后更新方便，与原版本保持同步，及时修复因依赖引入的bug。

1、查看是否可以`CocoaPads`依赖

	$ pod search libname

2 、git submodule

* a、添加一个新的库依赖，如添加nimbus框架

{% highlight bash %}
$ git submodule add https://github.com/jverkoey/nimbus.git vendor/nimbus
{% endhighlight %}

* b、已有库依赖，初始化和更新
当从github clone一个项目时，发现如果有.gitmodule文件，则说明有git submodule依赖，则需要做一下两步操作：

{% highlight bash %}
$ git submodule init & git submodule update
{% endhighlight %}

3、CocoaPods使用参考：
[http://blog.devtang.com/blog/2012/12/02/use-cocoapod-to-manage-ios-lib-dependency/https://github.com/CocoaPods/CocoaPods](http://blog.devtang.com/blog/2012/12/02/use-cocoapod-to-manage-ios-lib-dependency/https://github.com/CocoaPods/CocoaPods)