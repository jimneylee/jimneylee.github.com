---
author: jimneylee
layout: post
title: "iOS7 修改导航栏和状态栏背景"
date: 2013-11-25 10:07:08
comments: true
tags:
- ios7
- barstyle
---

如果我们APP的主色调偏深，那就需要修改导航栏（navigationbar）和状态栏（statusbar）背景及字体颜色

1、修改导航栏字体和背景

	[[UINavigationBar appearance] setTintColor:[UIColor whiteColor]];//字体
	[[UINavigationBar appearance] setBarTintColor:[UIColor redColor]];//背景

2、修改状态栏字体白色

a、plist中添加`UIViewControllerBasedStatusBarAppearance`字段并设置为NO，默认YES

b、`AppDelegate`中添加如下代码：

	[[UIApplication sharedApplication] setStatusBarStyle:UIStatusBarStyleLightContent];