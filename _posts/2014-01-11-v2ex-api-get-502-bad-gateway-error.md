---
layout: post
title: "ios http请求出现502-bad-gateway错误"
tagline: ""
description: ""
category: 
tags: 
- iphone
- nginx
---
{% include JB/setup %}

当我试着在[Ruby China社区的iPhone客户端](https://github.com/jimneylee/JLRubyChina-iPhone)基础上，兼容[v2ex社区](http://www.v2ex.com/)的[Project Babel api接口](https://github.com/livid/v2ex/blob/master/api.py)。
请求遇到502-bad-gateway错误，一开始以为是使用[AFNetworking](https://github.com/AFNetworking/AFNetworking)的问题，我通过NSString同步获取，发现也没获取到，所以我推测应该是跟后台基于nginx反向代理有关。

header输出结果比较：

1、使用AFNetworking框架底层设置的User-Agent默认值
整个请求头如下:

	{
	    Accept = "application/json";
	    "Accept-Language" = "en;q=1, fr;q=0.9, de;q=0.8, zh-Hans;q=0.7, zh-Hant;q=0.6, ja;q=0.5";
	    "User-Agent" = "JLV2EX/1.0 (iPhone Simulator; iOS 7.0.3; Scale/2.00)";
	}
	
请求报错:502 bad gateway :(

2、通过手机浏览器打开[http://whatsmyuseragent.com/](http://whatsmyuseragent.com/)来获取到`User-Agent`，再赋给header的`User-Agent`
整个请求头，

	{
	    Accept = "application/json";
	    "Accept-Language" = "en;q=1, fr;q=0.9, de;q=0.8, zh-Hans;q=0.7, zh-Hant;q=0.6, ja;q=0.5";
	    "User-Agent" = "Mozilla/5.0 (iPhone; CPU iPhone OS 7_0_3 like Mac OS X) AppleWebKit/537.51.1 (KHTML, like Gecko) Version/7.0 Mobile/11B508 Safari/9537.53";
	}
	
可以顺利获取信息 :)

网上google了一通，找到如下解决方案。特此记录。参考SO帖子：
[http://stackoverflow.com/questions/8487581/uiwebview-ios5-changing-user-agent/8666438#8666438](http://stackoverflow.com/questions/8487581/uiwebview-ios5-changing-user-agent/8666438#8666438)

	//RCAPIClient继承于AFHTTPClient，做如下修改：
	- (id)initWithBaseURL:(NSURL *)url
	{
	    self = [super initWithBaseURL:url];
	    if (self) {
	        self.parameterEncoding = AFJSONParameterEncoding;
	        
	        // 502-bad-gateway error, set user agent from http://whatsmyuseragent.com/
	        // http://stackoverflow.com/questions/8487581/uiwebview-ios5-changing-user-agent/8666438#8666438
	        if (ForumBaseAPIType_V2EX == FORUM_BASE_API_TYPE) {
	            NSString* testUserAgent = @"Mozilla/5.0 (iPhone; CPU iPhone OS 7_0_3 like Mac OS X) AppleWebKit/537.51.1 (KHTML, like Gecko) Version/7.0 Mobile/11B508 Safari/9537.53";
	            [self setDefaultHeader:@"User-Agent" value:testUserAgent];
	        }
	    }
	    return self;
	}
	
代码见：[https://github.com/jimneylee/JLRubyChina-iPhone/blob/master/JLRubyChina/Common/RCAPIClient.m](https://github.com/jimneylee/JLRubyChina-iPhone/blob/master/JLRubyChina/Common/RCAPIClient.m)的initWithBaseURL方法