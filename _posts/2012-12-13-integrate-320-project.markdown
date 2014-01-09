---
author: jimneylee
layout: post
title: "记：Three20工程与非Three20工程的集成问题"
date: 2012-12-13 16:20:18
comments: true
tags:
- Three20
- xcode
---

这两天需要把我的基于`Three20`的工程打一个静态库供客户方使用。因为设计到第三方库的共用，出现了很多问题。

问题1、`SBJSON`第三方库使用的冲突

由于`three20`的json解析框架与客户使用版本有些差异，并且严重的问题是支付宝的静态库竟然把SBJSON一并打入静态库，同时需要用到`SBJSON.h`头文件。我这心抓挠的闷。没办法，我只能把320的json静态库，重新打包，不包含.m文件，同时加入`SBJSON.h`，重新生成静态库，供对方使用才解决了这个问题。
（PS：奇怪的是，支付宝官方说没打入静态库，需要使用者自己添加，但是添加了就出现`duplicate symbol`错误）

问题2、客户如何调用我主控制器

开始我以为很简单，一句话的事：   

    [[TTNavigator navigator] openURLAction:[TTURLAction actionWithURLPath:@"mine://maintab"]];

但是我发现返回不了。看了底层才知道，由于three20基于导航url思想，默认会生成一个               `TTNavigationController`，并设为rootViewController，见`TTBaseNavigator.m` line242:

    if (nil != _rootContainer) {
      [_rootContainer navigator:self setRootViewController:_rootViewController];
    } else {
      [self.window addSubview:_rootViewController.view];
    }

如果直接通过window来添加，效果不好。此处rootContainer的代理使用很特别，哈哈，通过它能使three20工程和其他项目工程无缝链接。代码如下：

     - (void)startLibraryWithParentController:(UIViewController*)parentController
    {
        self.parentViewController = parentController;
        // 设置容器的代理，显示320的rootViewController
        [[TTNavigator navigator] setRootContainer:self];
       // 此处一般在AppDelegate 调用，此处省略了Three20工程初始化初始化[AppDelegate init]
        [[TTNavigator navigator] openURLAction:[TTURLAction actionWithURLPath:@"mine://maintab"]];
    }
    - (void)stopLibrary
    {
        [self.parentViewController dismissModalViewControllerAnimated:YES];
        [[TTNavigator navigator] removeAllViewControllers];
        [delegate applicationWillTerminate:nil];    
        // release all singleton class if need
    }
    ///////////////////////////////////////////////////////////////////////////////////////////////////
    #pragma mark -
    #pragma mark TTNavigatorRootContainer
    - (void)navigator:(TTBaseNavigator*)navigator setRootViewController:(UIViewController*)controller {
        // 对方的父控制器上显示Three20的rootViewController
        [self.parentViewController presentModalViewController:controller animated:YES];
    }
