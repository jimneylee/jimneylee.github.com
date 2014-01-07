---
author: jimneylee
layout: post
title: "开源基于nimbus的新浪微博iPhone客户端框架"
date: 2013-11-04 10:40
comments: true
tags:
- sinaweibo
- nimbus
- opensource
---

基于轻量级iOS开发框架[nimbus](https://github.com/jverkoey/nimbus)，网络层采用[AFNetworking](https://github.com/AFNetworking/AFNetworking)，

在此基础上进行二次构建，可以简单、便捷地处理和显示列表数据，

通过制作iOS7上新浪微博APP的首页，介绍框架的使用，通过开源分享，一起交流进步。

主要分享的技术点如下：

1、二次构建，简化tableView网络数据请求和显示

2、类似官方APP富文本的布局和关键字的识别和交互

3、发布微博、拍照及获取地理位置

4、微博列表中查看原图功能

PS:以前项目中主要使用[three20](https://github.com/facebook/three20)开发APP，了解过three20的同学，应该比较熟悉nimbus的作者，不熟悉请google之。

![](https://github-camo.global.ssl.fastly.net/aa54075a5758fca0eb7e592756b6310849e62f3a/687474703a2f2f63632e636f63696d672e636f6d2f6262732f6174746163686d656e742f4669645f31392f31395f32323433355f3963373762363637303761646231352e676966)
![](https://github-camo.global.ssl.fastly.net/affc16164ff3df8904111c899a5616b6ffef820f/687474703a2f2f6769742e6f736368696e612e6e65742f6a696d6e65796c65652f53696e614d426c6f674e696d6275732f7261772f6d61737465722f53696e614d426c6f672f496d616765732f53637265656e73686f742f706f73746e65777374617475732e706e67)
![](https://github-camo.global.ssl.fastly.net/4d8453e9cf6c1f569aac8059dd320feb834a9c5e/687474703a2f2f6769742e6f736368696e612e6e65742f6a696d6e65796c65652f53696e614d426c6f674e696d6275732f7261772f6d61737465722f53696e614d426c6f672f496d616765732f53637265656e73686f742f7265706f73742e706e67)