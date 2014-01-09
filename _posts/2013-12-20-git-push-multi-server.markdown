---
author: jimneylee
layout: post
title: "git如何同步到多个git server"
date: 2013-12-20 10:13:17
comments: true
tags:
- git
- github
- gitcafe
---

打算把我在[github](https://github.com/)开源的[RubyChina](http://ruby-china.org/)的[iPhone客户端](https://github.com/jimneylee/JLRubyChina-iPhone)，同时发布到osc(开源中国)和gitcafe。想更多的同学参与进来，交流和学习，同时我认为这也是一件有兴趣和意义的事。对自己的技术分享也有很大的帮助。

初步有这个想法（需求），开始尝试有没有这个可能，如何简单方便，同步多个仓库。得意于git的分布式特点，本地仓库与服务器是平等的，包括所有commit logs。也就是说不管提交到哪个git server，对本地仓库而言，只是多了一份clone，所以往多个git server提交时，只要修改.git/config，即可很方便地进行同步更新。

但是如果每次要提交不同的server，都要手工修改这个config，还是比较麻烦，而且容易出错。所以我只要写几个sh脚本，每次执行以下自动修改就很方便了。

具体操作如下：

* step1: 拷贝github账户的SSH公钥，到其他账户，保持一样
* step2: 在.git目录下cp多个config文件，命名如下：
	* github:  config_github
	* gitcafe: config_gitcafe	
	* osc:     config_osc

* step3:在.git目录下`toch`多个sh执行文件(`chmod a+x file`)
	* github:  use_github.sh
	* gitcafe: use_gitcafe.sh
	* osc:     use_osc.sh
	
使sh文件均可被执行：`chmod a+x use*.sh`
如想同步到gitcafe上，只需要到.git目录下执行：

	$ ./use_gitcafe.sh
	$ git push origin master

OK！
在同步到gitcaft，我遇到这个问题：

>fatal: The remote end hung up unexpectedly
>Everything up-to-date

有可能是你一次提交的文件太大，需设置postBuffer大小，我需要50MB，所以：

	$ git config http.postBuffer 52428800
	
或

	$ git config https.postBuffer 52428800

参见SO：[http://stackoverflow.com/questions/12651749/git-push-fails-rpc-failed-result-22-http-code-411](http://stackoverflow.com/questions/12651749/git-push-fails-rpc-failed-result-22-http-code-411)