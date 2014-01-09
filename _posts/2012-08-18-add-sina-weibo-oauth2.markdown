---
author: jimneylee
layout: post
title: "为ShareKit添加Sina Weibo OAuth2.0授权和分享功能 "
date: 2012-08-18 23:12
comments: true
tags:
- oauth2
- weibo
- iphone
---

 继[新浪微博开发平台](http://open.weibo.com/)推出OAuth2.0认证，新注册的app只能通过新的认证授权接口来实现登录交互。我在国内大牛icyleaf[https://github.com/icyleaf/ShareKit](https://github.com/icyleaf/ShareKit)的fork原始sharekit基础上,并参考了国外另一个大牛fork[https://github.com/ShareKit/ShareKit](https://github.com/ShareKit/ShareKit)fork实现国外OAuth2.0思路和代码，添加了仅针对于国内新浪微博的OAuth2.0功能（由于中国国内开放平台引入国外新技术的“创意”改造，我暂未考虑实现通用2.0接口实现方法。OAuth1.0这方面引进风范，腾讯微博做的尤为特色），同时保留原来的OAuth1.0功能。归功于原作者大师级别的架构，添加OAuth2.0功能还是很方便。

如果你之前已经用了sharekit并做了自己的修改，集成新浪微博OAuth2.0分享需要仅注意以下几点：（so simple）

1、原有代码整体改动不大，添加如下V2文件按夹

![](http://www.cocoachina.com/bbs/attachment/thumb/Fid_19/19_22435_2407305e251b112.png)

2、`OAAsynchronousDataFetcher`增加一个接口`- (void)startNoPrepare`，去除`prepare`方法，OAuth2.0不需要通过HMAC-SHA1生成signature
3、在`SHKSharers.plist`添加`SHKSinaWeiboV2`，同时在`SHKConfig.h`

	// Sina Weibo 2.0
	#define SHKSinaWeiboV2ConsumerKey         @"1383003023"    // The consumer key
	#define SHKSinaWeiboV2ConsumerSecret      @"3e6c2fa689fbcf0aad7418f2dc8093ef"    // The secret key
	#define SHKSinaWeiboV2CallbackUrl         @"https://api.weibo.com/oauth2/default.html"    // You need to set this if 

![](http://www.cocoachina.com/bbs/attachment/thumb/Fid_19/19_22435_aa2c02d6a43dfc1.png)

具体参考代码，分享如下链接，支持开源，enjoy it。
Note：目前腾讯微博仅支持文字分享，图片分享还未实现，欢迎补充！

---
update(12-09-05):
修改授权页面的url，需要添加&display=mobile，不然显示的是网页版的授权页面。
修改地方为SHKSinaWeiboV2.m 的 promptAuthorization方法

	NSString* urlStr = [NSString stringWithFormat:@"%@?client_id=%@&response_type=code&redirect_uri=%@&display=mobile", authorizeURL, self.consumerKey, [self.authorizeCallbackURL.absoluteString URLEncodedString]];
