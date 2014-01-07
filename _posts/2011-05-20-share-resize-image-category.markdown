---
author: jimneylee
layout: post
title: "UIImage的一个category：缩放图片限制横宽区域内"
date: 2011-05-20 13:56
comments: true
tags:
- iphone
- UIImage
---

{% highlight objc %}
/*
 * 缩放图片限制横宽区域内
 */
- (UIImage*)scaleWithSize:(CGSize)size {
    CGFloat imageWidth = self.size.width;
    CGFloat imageHeight = self.size.height;
 
    CGFloat wScale = imageWidth / size.width;
    CGFloat hScale = imageHeight / size.height;
    NSLog(@"ws = %f, hs = %f", wScale, hScale);
     
    CGSize newSize = self.size;
    BOOL needRedraw;
    if (wScale > hScale && wScale > 1) {
        newSize.width = size.width;
        newSize.height = imageHeight / wScale;
        needRedraw = YES;
    }
    else if (hScale > wScale && hScale > 1) {
        newSize.width = imageWidth / hScale;
        newSize.height = size.height;
        needRedraw = YES;
    }
     
    NSLog(@"w = %f, h = %f", newSize.width, newSize.height);
     
    if (needRedraw) {
        UIGraphicsBeginImageContext(newSize);
        [self drawInRect:CGRectMake(0, 0, newSize.width, newSize.height)];
        self = UIGraphicsGetImageFromCurrentImageContext();
        UIGraphicsEndImageContext();
    }
 
    return self;
}
{% endhighlight %}