---
author: jimneylee
layout: post
title: "XCode4.6.2编译安装到iOS7"
date: 2013-10-21 10:56:44
comments: true
tags:
- ios7
- xcode
---

由于老的工程使用XCode5.0编译到iOS7设备上，总会有莫名其妙的问题，所以一直想法设法仍然通过XCode4.6.2编译安装。经过测试，完美解决了问题。
简要步骤：
1、下载最新的XCode5.x的dmg安装文件，安装后在应用程序目录单独存放如下图：
![](http://s12.sinaimg.cn/mw690/6d2b4810gx6DALTwJm31b&690)

2 、右击工程文件，用XCode5打开，并运行安装到您的设备上
3、右击工程文件，用XCode4.6.2开发，并运行安装到模拟器上
4、哈哈，用XCode4.6.2选择设备，你会看到设备已经可选
![](http://s10.sinaimg.cn/mw690/6d2b4810gx6DAMnGl7jb9&690)

不用感谢我，感谢老外的分享吧，我只是传道士。enjoy it！

以下来自老外原文来自stackoverflow：[http://stackoverflow.com/questions/17075894/deploy-from-xcode-4-6-2-to-ios-7-beta-device](http://stackoverflow.com/questions/17075894/deploy-from-xcode-4-6-2-to-ios-7-beta-device)

>Well I dont know If this is of any help to anyone but me. But I have been able to use Xcode 4.6.2 to deploy to my iPhone 5 running iOS 7. I think it is due to a bug in the system but it doesnt matter to me. It works ok. Now to do this, I do as follows:

>Make sure you have the latest version of Xcode from the App Store. (Dont know why, but why not?)

>Download and Install Xcode 5.

>Close all instances of Xcode running in your system (4.6.2 and 5)

>Run Xcode 5. you will see it recognises your device, you probably have to activate it as use it for development again.

>Run Xcode 4.6.2 simultaneously. You will see it recognises your iPhone as in: make it valid target for development.

>close or do whatever you want with Xcode 5. From this point onwards You can keep using xcode 4.6.2

>I havent turned my computer off or restarted it in a long time so I dont know if this is a fluke or what. But other people I work with have been able to do the same, so I expect it to work for you.

>EDIT:

>Better yet. Something I have found useful is building from Xcode 4.6.x to an iOS 7 device, actually makes the phone run it in iOS6 or before Mode which is the way all apps run at the moment. So my guess is that this would be what your app would look like in iOS 7 if deployed from the app store. Assuming you are targeting iOS 4+

>Similarly, if you build the same app using Xcode 5, it tries to incorporate some iOS 7 appearance proxies by default and certainly the ui behaves differently. Granted I havent played with Xcode 5 much, there is probably a toggle somewhere to turn this compatibility mode on and off.