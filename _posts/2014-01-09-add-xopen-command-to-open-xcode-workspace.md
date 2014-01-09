---
layout: post
title: "自定义xopen命令打开工程"
description: ""
category: 
tags: [xcode ruby]
---
{% include JB/setup %}

通常我们在使用CocoaPods命令'[sudo] pod update'安装好依赖库，会自动生成`projectname.xcworkspace`，通过打开workspace来正确打开所有工程。在终端使用系统的open命令打开，我们要细心注意后缀`projectname.xcodeproj`，还是`projectname.xcworkspace`，这样很麻烦。
通过下面这个简单的ruby脚本，只需要xopen projectname就可以优先打开`projectname.xcworkspace`。

	#!/usr/bin/env ruby
	 
	require 'shellwords'
	 
	proj = Dir['*.xcworkspace'].first
	proj = Dir['*.xcodeproj'].first unless proj
	 
	if proj
	  puts "Opening #{proj}"
	  `open #{proj}`
	else
	  puts "No xcworkspace|xcproj file found"
	end

把这个文件拷贝到`/usr/local/bin`目录下，并添加权限：`chmod 777 xopen`

参考：[http://www.marcelofabri.com/xopen/](http://www.marcelofabri.com/xopen/)