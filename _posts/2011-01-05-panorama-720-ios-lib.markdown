---
author: jimneylee
layout: post
title: "赞一个开源库：720全景显示"
date: 2011-01-05 17:00
comments: true
tags:
- iphone
- panorama
---

展示的效果跟香港720一样，感谢老外的开源精神。

google代码首页：

[http://code.google.com/p/panoramagl/](http://code.google.com/p/panoramagl/)

库使用详细介绍：

[http://www.codeproject.com/KB/iPhone/panoramagl.aspx](http://www.codeproject.com/KB/iPhone/panoramagl.aspx)

功能介绍：

* 1、支持720全景展示
* 2、支持重力加速度感应控制

遇到主要问题及解决方法：

1、报错如：
>No architectures to compile for (ARCHS=armv6 armv7, VALID_ARCHS=i386 ppc ppc64 ppc7400 ppc970 x86_64).

需要添加armv6和armv7，如下图：
![image](http://www.cocoachina.com/bbs/attachment/Fid_19/19_22435_8c902a076a516d0.png)

2、`glu.h`链接文件出错，需要`include "glues.h"`

	#ifndef __glu_h__
	#define __glu_h__
	#include "glues.h"
	#endif

3、报错如：
>Undefined symbols:
"_OBJC_CLASS_$_PLTexture", referenced from:
objc-class-ref-to-PLTexture in HelloPanoramaViewController.o
ld: symbol(s) not found

一般这个问题由于添加`PanoramaGL.xcodeproj`到`HelloPanorama`项目中时，忘了勾选库的右上角的圆框，如下图：

![http://www.cocoachina.com/bbs/attachment/Fid_19/19_22435_10c6f841de495b0.png](http://www.cocoachina.com/bbs/attachment/Fid_19/19_22435_10c6f841de495b0.png)

也就是stackoverflow上这句话的意思，但是因为没图，其他人看了很不理解，包括我 :(
>I had the same problem, and noticed that 
although I included PanoramaGL xcode 
project, **its lib (libPanoramaGL.a) was not 
selected to build with the target (that >little checkbox in the top right list of >XCode)**.

其实这句话意思是：这个.a静态库没有被选中，导致不能被编译到目标程序中。
右键.a文件，`get info -->Targets`，多targets项目，可以指定一个静态库为那几个targets所有，同样的道理使用于程序源文件和资源文件，看了下面这幅图就更加理解了
![http://www.cocoachina.com/bbs/attachment/Fid_19/19_22435_bd730eeb69cb7f6.png](http://www.cocoachina.com/bbs/attachment/Fid_19/19_22435_bd730eeb69cb7f6.png)