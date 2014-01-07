---
author: jimneylee
layout: post
title: "记录一个低级错误"
date: 2010-11-19 04:54
comments: true
tags:
- iphone
- debug
---
  程序中需要在视图上重复添加新的子层`CATiledLayer`类对象来显示不同的pdf文件，因为层对象采用`[CATiledLayer layer]`静态方法来获取内存，心里想当然认为子层对象的生命周期不需要我再管了，每次都采用此静态方法产生新的子层对象，然后添加到当前视图的根层上。但是下班前突然发现打开七八张pdf文件后，机子就罢工，显示内存警告然后翘辫子了。
第一时间我就想到`leak`工具，但是悲剧的是，并没有内存泄露提示。接着google了半夜，看了千层帖，终无果！浑天暗地之际，突然醒悟，`addSublayer`到父层，会对子层retain一次，所以每次新开辟前，需要`[tiledLayer removeFromSuperlayer]`来释放前一次的层对象。
总结：

* 1、写程序是穿线的活，心细不能想当然。
* 2、leak工具不是万能，关键时还需冷静分析。
* 3、调试不是自己写的程序，很费神。
* 4、头昏时就休息，不然效率低下，而且伤身体。

PS：不是因为紧急，偶是不会熬通宵，这是偶上班来的处女夜，竟然竟为此差错，不谈也罢，只为纪念！欢迎拍砖！
cc地址：[http://www.cocoachina.com/bbs/read.php?tid=38540](http://www.cocoachina.com/bbs/read.php?tid=38540)