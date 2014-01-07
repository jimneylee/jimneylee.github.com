---
author: jimneylee
layout: post
title: "解决可恶的Code Sign error"
date: 2012-11-20 16:39:37
comments: true
tags:
- sign
- iphone
- xcode
- key
---

这两天的时间都浪费搞证书上了。不知咋的，突然证书无法使用，开始是`valid signing certificate not found`，同时XCode里面证书设置那边报`key/certificate`不匹配。

昨天摸索一天，终于找到必杀技。revoke掉certificate，重新生成所有的证书，才没有上面的问题。

今天又遇到下面这个DT的问题：
`Code Sign error: Certificate identity 'iPhone Developer: 
My Name (xxx)' appears more than once in the keychain. 
The codesign tool requires there only be one.`

我试了如下方面，对我没有可能的对你有用。
1、因为反复安装证书，到`access key（钥匙串访问）`删除相同的证书。
2、删除掉过期的证书。
3、记得关闭xcode和`access key（钥匙串访问）`
4、这个很重要。跟1有关系。就是你删除相同的证书，但是实际没有删除，这个是acces key的bug。

下面是苹果官方的解决方法，请参考：
[https://developer.apple.com/library/ios/#technotes/tn2250/_index.html#//apple_ref/doc/uid/DTS40009933-CH1-TNTAG3](https://developer.apple.com/library/ios/#technotes/tn2250/_index.html#//apple_ref/doc/uid/DTS40009933-CH1-TNTAG3)