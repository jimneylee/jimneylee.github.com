---
author: jimneylee
layout: post
title: "解决iOS7下，NSURLRequest设置NSURLCacheStorageNotAllowed的缓存问题"
date: 2013-10-05 21:29:02
comments: true
tags:
- ios7
- NSURLRequest
- cache
---

	NSURLRequest *request = [NSURLRequest requestWithURL:url											  cachePolicy:NSURLCacheStorageNotAllowed    
				 		  				  timeoutInterval:20.f];

最近发现已发布的app的天气每天获取的都是以前某一天的数据，但是之前没有这个情况，原来ios6下没有问题，但是在ios7下，即使设置了`NSURLCacheStorageNotAllowed`，仍然缓存旧数据的问题。

根据SO[这个帖子](http://stackoverflow.com/questions/18923675/nsurlrequest-cache-issue-ios-7)可以找到答案：
两种解决方案：
 
1、每次请求之前先删除旧的缓存

	[[NSURLCache sharedURLCache] removeCachedResponseForRequest:request];

2、cachePolicy设置为0，原因不明，先留个备份。
我暂时采用2中方法，简单只是为了效率。后面再深究，先更新app吧，蛋疼，不知今天几个小时能上传成功。