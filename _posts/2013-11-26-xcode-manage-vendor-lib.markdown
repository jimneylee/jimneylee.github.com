---
author: jimneylee
layout: post
title: "XCode第三方依赖库的管理（git submodule和CocoaPods）"
date: 2013-11-25 11:44:00
comments: true
tags:
- xcode
- git
- submodule
- cocoapods
---

采用`git submodule`和`CocoaPods`结合管理，如果依赖的库支持`CocoaPads`，优先采用`CocoaPads`，如果不支持，则采用`git submodule`。做依赖管理第三方库，主要为了日后更新方便，与原版本保持同步，及时修复因依赖引入的bug。

1、查看是否可以·CocoaPads·依赖

```bash
$ pod search libname
```

2 、git submodule

* a、添加一个新的库依赖，如添加nimbus框架

```bash
$ git submodule add https://github.com/jverkoey/nimbus.git vendor/nimbus
```

* b、已有库依赖，初始化和更新
当从github clone一个项目时，发现如果有.gitmodule文件，则说明有git submodule依赖，则需要做一下两步操作：

```bash
$ git submodule init & git submodule update
```

3、CocoaPods
参考：

[http://blog.devtang.com/blog/2012/12/02/use-cocoapod-to-manage-ios-lib-dependency/https://github.com/CocoaPods/CocoaPods](http://blog.devtang.com/blog/2012/12/02/use-cocoapod-to-manage-ios-lib-dependency/https://github.com/CocoaPods/CocoaPods)