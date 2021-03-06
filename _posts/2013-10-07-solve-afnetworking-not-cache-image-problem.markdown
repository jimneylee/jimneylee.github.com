---
author: jimneylee
layout: post
title: "解决图片缓存问题"
date: 2013-10-07 22:26:37
comments: true
tags:
- ios7
- nimbus
- AFNetworking
---

今天编译nimbus项目报错：ADDRESPONSE - not adding TO DISK OR MEMORY

	NSURLRequest *request = [NSURLRequest requestWithURL:url
											  cachePolicy:NSURLCacheStorageNotAllowed    
				 						  timeoutInterval:20.f];


今天使用`AFNetworking`库，`UIImaeView`缓存图片时，出现这个怪异的警告，图片无法disk缓存。
网上搜了一通，从下面这个帖子找到答案，原来是我的缓存限制了。
http://questiontrack.com/nsurlcache-disk-limit-1203248.html

nimbus官方网站上建议使用SDURL来管理硬盘缓存，代码提供为：

	// Nimbus implements its own in-memory cache for network images. Because of this we don't allocate
	// any memory for NSURLCache.
	static const NSUInteger kMemoryCapacity = 0;
	static const NSUInteger kDiskCapacity = 1024*1024*5; // 5MB disk cache
	SDURLCache *urlCache = [[[SDURLCache alloc] initWithMemoryCapacity:kMemoryCapacity
	                                                      diskCapacity:kDiskCapacity
	                                                          diskPath:[SDURLCache defaultCachePath]]
	                        autorelease];
	[NSURLCache setSharedURLCache:urlCache];

memory缓存被设置为0，故此处要修改为如下：

	// any memory for NSURLCache.
	static const NSUInteger kMemoryCapacity = 1024*1024*5; // 5MB disk cache
	static const NSUInteger kDiskCapacity = 1024*1024*20; // 5MB disk cache
	SDURLCache *urlCache = [[[SDURLCache alloc] initWithMemoryCapacity:kMemoryCapacity
	                                                      diskCapacity:kDiskCapacity
	                                                          diskPath:[SDURLCache defaultCachePath]]
	                        autorelease];
	[NSURLCache setSharedURLCache:urlCache];

希望对出现同样的问题同学提供及时的参考。