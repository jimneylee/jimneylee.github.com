---
author: jimneylee
layout: post
title: "iPhone 锁屏后事件的处理"
date: 2010-09-14 12:39
comments: true
tags:
- iphone
---
今天测试程序发现，在一代机上，按了最上面的屏保键，然后回到程序界面，机子就死掉了，不过ipad每这个问题。
请教：
这个问题是一代机子本身问题，还是我的程序问题？
如果是机子问题，我在程序中怎么避免，在哪边可以获得外界屏保设置的消息？不甚感谢！

PS（2010－09－16）:=============================================

之前的标题：iphone一代机，屏保后，程序死掉，机也完蛋
问题描述：
可能是我之前的描述有问题吧，其实我遇到的问题就是当人为按了最上面的锁屏键，解锁后回到程序时，一操作就会发生死机，郁闷一天！
后来经过分析调试，是主界面背景音乐的循环播放造成的，机器锁屏后，程序依旧运行，只是声音被系统暂停。
但是在我程序中，循环播放的背景音乐，因为锁屏、解锁，系统暂停再开启就会导致系统挂掉。
解决思路： 
既然是因为锁屏，系统暂停我程序音乐播放导致的，而我程序内部对声音的暂停和恢复是没问题的。
所以我最初的思路就是不让系统暂停我的背景声音，通过获得锁屏后的事件，程序自己来暂停声音，然后锁屏后再恢复播放。沿着这个思路，找到这个触发的事件是王道。

未完待续，肚子催我回去了，明天补充！

PS (2010-09-17):================================================

[深入理解iPhone委托模式兼谈iPhone生命周期](http://www.cocoachina.com/iphonedev/sdk/2010/0611/1670.html)

从cc这篇文章中我了解到（PS：感谢文章作者，同时也非常感谢版主们的辛苦整理），程序的主代理`UIApplicationDelegate`接口提供给生命周期函数来处理应用程序以及应用程序的系统事件，也就是说`UIApplicationDelegate`代理能够处理系统发给应用程序的消息，那么我要找的锁屏后系统触发的事件，应该在这个主代理所能处理的生命周期函数中处理。

通过log输出发现，下面几个函数中能够输出信息：

    - (void)applicationWillBecomeActive::(UIApplication *)application;
    - (void)applicationWillResignActive:(UIApplication *)application;

    - (void)applicationDidBecomeActive:(UIApplication *)application;
    - (void)applicationDidResignActive:(UIApplication *)application;
  从官方的SDK中得知，系统发送改变应用程序活动状态的事件，可以通过这两个函数处理。可以是一个来电或者SMS事件，当然还有我遇到的这个系统锁屏事件。
譬如：游戏正在运行，有来电了，你可以先暂停游戏，等通话结束，再恢复游戏。那我遇到的这个声音恢复导致死机问题，就可以自己来暂停和恢复背景声音。
问题是，我要在程序的主代理中`UIApplicationDelegate`处理这个事件很不方便，我的播放背景声音是在主Controller的子Controller中定义的，如果能在子Controller中处理这个事件，问题就容易多了。如何实现呢？

SDK文档这句话给了我启示：

>applicationWillResignActive:
Sent by the default notification center immediately before the application is deactivated.
A notification named NSApplicationWillResignActiveNotification. Calling the object method of this notification returns the NSApplication object itself.

`NSApplicationWillResignActiveNotification`是系统变成不活动（inactive）状态的消息，通过默认的notification center能够处理这个消息。对这个概念的理解，还得感谢CC的`babylon_0049`朋友乐于分享他自己的一些理解和分析，这在帮助我解决这个问题，启到了很大的提示。

[cc地址](http://www.cocoachina.com/bbs/read.php?tid=30971&page=1#198491)
同时关于`UIApplicationDidBecomeActiveNotification`，文档里有这句话： 

>when the device is locked, and gains focus when the device is unlocked

这使我解决问题的方向更加明确具体。

呵呵，到这里，解决方法就不言而喻了！
在自控制器或者视图的初始化内添加如下语句：

    //把self添加到NSNotificationCenter的观察者中
    [[NSNotificationCenter defaultCenter] addObserver:self
                                             selector:@selector(becomeActive) 
                                                 name:UIApplicationDidBecomeActiveNotification 
                                               object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self
                                             selector:@selector(resignActive) 
                                                 name:UIApplicationWillResignActiveNotification 
                                               object:nil];

响应的动作函数：

    /*
     *程序活动的动作
     */
    - (void)becomeActive
    {
        //创建新的定时器
        //继续播放声音，stop后currentTime值不变，直接使用play
        //TODO:
    }    
    /*
     *程序不活动的动作
     */
    - (void)resignActive
    {
        //关闭定时器
        //停止背景播放声音，个人测试发现最好使用stop方法
        //TODO:
    }
与大家一起分享，希望能给没有接触这部分知识的朋友有一点帮助！
cc地址：[http://www.cocoachina.com/bbs/read.php?tid=32245](http://www.cocoachina.com/bbs/read.php?tid=32245)