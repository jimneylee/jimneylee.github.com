---
author: jimneylee
layout: post
title: "记：ios PLCrashReporter 学习一"
date: 2012-12-18 10:10:14
comments: true
tags:
- PLCrashReporter
- xcode
---

经过昨天今天的探索，好不容易把PLCrashReporter给跑起来。
下面分享下步骤，
1、项目主页：[http://code.google.com/p/plcrashreporter](http://code.google.com/p/plcrashreporter)

* a. git clone源码，[http://code.google.com/p/plcrashreporter/source/checkout](http://code.google.com/p/plcrashreporter/source/checkout)
* b. 向服务器后台发送崩溃测试demo位于工程下的`contrib`，`php-crashreporter-demo`。
* c. 目前我还没能直接从源码哪里生成ios的framework，所以直接在Downloads里面下载了官方编译好的framework，`PLCrashReporter-1.1-rc2.dmg`（目前[最新版本](http://code.google.com/p/plcrashreporter/downloads/list)），可以用的。如果您能编译，请留言分享。
    
2、遇到一个诡异的问题，生成了crash，但是没有生成崩溃日志文件的问题
    官方issue见此处：[http://code.google.com/p/plcrashreporter/issues/detail?id=32](http://code.google.com/p/plcrashreporter/issues/detail?id=32)
    内容如下：

>gdb will catch the mach exception
>before it hits PLCrashReporter's signal handler; you have to
>execute the program outside of the debugger to test the crash
>handling behavior.

我在测试demo时，xcode中编译运行，此时gdb调试进程会在`PLCrashReporter`之前捕捉，导致PL没有获取异常信息。所以请直接在模拟器或者设备中，运行崩溃捕捉测试。

客户端的崩溃报告生成了。后面我将学习配置server端，存储崩溃报告信息和使用`plcrashutil`命令行工具查看客户端提交的崩溃日志信息。