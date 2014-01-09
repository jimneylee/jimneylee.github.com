---
author: jimneylee
layout: post
title: "CocoaPods问题出错"
date: 2013-11-11 13:49:53
comments: true
tags:
- cocoapods
---

CocoaPods问题：`Unable to activate xcodeproj-0.14.1`
出现下面这个问题，由于在`pod install`，未使用`sudo`权限去执行，有时候犯的错误就是2，分享一下，不想其他同学也为这个问题浪费不必要的时间。

>/Users/jimney/.rvm/rubies/ruby-2.0.0-p0/lib/ruby/site_ruby/2.0.0/rubygems/specification.rb:1990:in `raise_if_conflicts': Unable to activate xcodeproj-0.14.1, because activesupport-4.0.0.rc1 conflicts with activesupport (~> 3.0) (Gem::LoadError)
from /Users/jimney/.rvm/rubies/ruby-2.0.0-p0/lib/ruby/site_ruby/2.0.0/rubygems/specification.rb:1163:in `activate'
from /Users/jimney/.rvm/rubies/ruby-2.0.0-p0/lib/ruby/site_ruby/2.0.0/rubygems/specification.rb:1199:in `block in activate_dependencies'
from /Users/jimney/.rvm/rubies/ruby-2.0.0-p0/lib/ruby/site_ruby/2.0.0/rubygems/specification.rb:1185:in `each'
from /Users/jimney/.rvm/rubies/ruby-2.0.0-p0/lib/ruby/site_ruby/2.0.0/rubygems/specification.rb:1185:in `activate_dependencies'
from /Users/jimney/.rvm/rubies/ruby-2.0.0-p0/lib/ruby/site_ruby/2.0.0/rubygems/specification.rb:1167:in `activate'
from /Users/jimney/.rvm/rubies/ruby-2.0.0-p0/lib/ruby/site_ruby/2.0.0/rubygems/core_ext/kernel_gem.rb:48:in `gem'
from /Users/jimney/.rvm/gems/ruby-2.0.0-p0/bin/pod:22:in