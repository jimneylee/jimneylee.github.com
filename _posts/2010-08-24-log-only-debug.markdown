---
author: jimneylee
layout: post
title: "调试信息只在debug模式下输出"
date: 2010-08-24 11:14
comments: true
category: debug
tags:
- xcode
- debug
- log
---

app发布后，程序运行过程中尽量不要有调试log信息输出，因为这样会影响程序运行的效率。通过宏定义设置，使得程序只在debug模式下输出这些只对于我们开发者有用的信息，而release时不会输出。
一、设置步骤如下：
1、首先建立一个宏定义文件，在其中加入如下代码:

	//! 1、XCode中设置控制
	// Target > Get Info > Build > GCC_PREPROCESSOR_DEFINITIONS
	// Configuration = Release: <empty>
	//               = Debug:   DEBUG_MODE=1
	//！2、人为控制
	//#define DEBUG_MODE 
	#ifdef DEBUG_MODE
	#define DebugLog( s, ... ) NSLog( @"<%p %@:(%d)> %@", self, [[NSString stringWithUTF8String:__FILE__] lastPathComponent], __LINE__, [NSString stringWithFormat:(s), ##__VA_ARGS__] )
	#else
	#define DebugLog( s, ... ) 
	#endif

2、在Target的Bulid搜索 GCC_PREPROCESSOR_DEFINITIONS（或者preprocessor macros），没有自己创建一个
3、选择 左上角的Configuration的 Debug，在左下角的下拉框选择->Edit Definition at this Level ,添加 DEBUG_MODE=1
4、选择左上角的Configuration: Release，确认没有设置 DEBUG_MODE 的值

二、使用：
首先相应有调试信息的文件中包含此宏文件，使用DebugLog替代cocoa的NSLog,格式化输出的方式一样。

PS：这样我们就可以做到log调试信息在release时不会输出，同时省去了人为疏忽，发布时忘了修改调试标志！
[参考SO原文地址](http://stackoverflow.com/questions/300673/is-it-true-that-one-should-not-use-nslog-on-production-code )