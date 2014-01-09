---
author: jimneylee
layout: post
title: "CocoaPods出错"
date: 2013-11-10 13:49:53
comments: true
tags:
- cocoapods
---

CocoaPods问题：`diff: /../Podfile.lock: No such file or directory`
解决方法转自此处：[http://www.cnblogs.com/ios-wmm/p/3360958.html](http://www.cnblogs.com/ios-wmm/p/3360958.html)

工程使用`CocoaPods`管理第三方库，在新的目录update版本的时候出现如下问题
 
* 问题1描述：

>diff: /../Podfile.lock: No such file or directory diff: /Manifest.lock: No such file or directory error: The sandbox is not in sync with the Podfile.lock. Run 'pod install' or update your CocoaPods installation.
 
解决办法：
进入到工程目录重新`pod install`一下
 
上面步骤进行过之后运行工程可能会有下面错误，那是因为当前用户的权限所致。

* 问题2描述：

>/Users/wmm-mac/Documents/Program-SVN/Versions/code/iPhone/GeneralProject/Pods/Pods-resources.sh: line 5: /Users/wmm-mac/Documents/Program-SVN/Versions/code/iPhone/GeneralProject/Pods/resources-to-copy-GeneralProject.txt: Permission denied
 
Pod没有权限：
如果没有权限，可执行下面代码

	$ sudo chmod 777 Pods
