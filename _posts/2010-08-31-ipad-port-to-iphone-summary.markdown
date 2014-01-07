---
author: jimneylee
layout: post
title: "项目从iPad转成iPhone完成总结"
date: 2010-08-31 11:20
comments: true
tags:
- xcode
- iphone
- port
---

   今天出现了令人头疼的现象，我在横竖转换时，在`- (void)willAnimateFirstHalfOfRotationToInterfaceOrientation:
(UIInterfaceOrientation)toInterfaceOrientation 
duration:(NSTimeInterval)duration`函数中，做了视图setFrame操作，但是发现视图并没有被准确地放到对应位置，发生很大偏移，而且有的视图我根本被对hidden属性进行设置，然而却忽儿出现，忽儿没了！

   花了几天时间终于将我的ipad上的app转到iphone一代机上了，现在为自己做一个总结，因为是第一次，过程中也遇到不少问题，横竖屏切换过程，时常会给我莫名的异常。

* 1、ipad使用的是sdk3.2，建立的工程基于ipad的UIView项目，如何能高效地转成iphone程序呢，依靠XCode本身的项目转换，偶还不知道，只知道iphone转ipad程序，可以通过targe中upgrade current tartget for ipad，但是也不是很了解。我采用的是贴代码方式，新建一个基于iphone的UIView项目，然后将所有的代码文件和资源文件复制到新的工程下，因为我的程序全部都是手工敲进去的所以也省事不少，而用nib做的，还需要把xib文件复制过去，这也是程序实现的一部份。因为我的ipad程序，没有用到sdk3.0之后的新特性，所以在iphone一代上可以直接运行了，那么我只要根据iphone的屏幕尺寸，重新做一份界面图片，替换掉。新建的工程注意修改sdk版本，双击target，bulid：base sdk->iPone device 3.0(保证跟真机一致)，Targeted device Family->iphone。

* 2、因为要实现横竖屏转换（之前ipad版没有），除了更换背景，还要重新设置上面子视图的位置。
横竖屏转换的事件通知在主控制器中如下代码实现：
{% highlight objc %}
- (BOOL)shouldAutorotateToInterfaceOrientation:(UIInterfaceOrientation)interfaceOrientation 
{
    // Return YES for supported orientations
    return YES;
}    
- (void)willAnimateSecondHalfOfRotationFromInterfaceOrientation:(UIInterfaceOrientation)fromInterfaceOrientation duration:(NSTimeInterval)duration 
{
    UIInterfaceOrientation toInterfaceOrientation = self.interfaceOrientation;    
    if (UIInterfaceOrientationIsPortrait(toInterfaceOrientation))
    {
        //控制器视图横屏设置
    }
    else//UIInterfaceOrientationIsLandscape(toInterfaceOrientation)
    {
        //控制器视图竖屏设置
    }
}
{% endhighlight %}
   对视图设置时，我个人建议如果视图大小不用改变则不用setFrame，直接修改视图的center，因为我发现某个视图如果你动画缩小，然后旋转setFrame，再执行动画放大此视图，不能执行，也就是说setFrame会与动画放缩冲突。