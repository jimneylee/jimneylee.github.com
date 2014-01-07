---
author: jimneylee
layout: post
title: "备份：版本管理过程中递归删除.svn和.git目录的命令"
date: 2012-12-20 16:35:06
comments: true
tags:
- scm
- git
- svn
- hightlight
---

* step1: 
{% highlight bash %}
$ find . -type d -name ".svn" | xargs rm -rf
{% endhighlight %}

* step2: 
{% highlight bash %}
$ find . -name .git -print0 | xargs -0 rm -rf
{% endhighlight %}
