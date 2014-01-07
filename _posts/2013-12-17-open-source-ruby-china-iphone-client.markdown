---
author: jimneylee
layout: post
title: "开源分享Ruby China社区的iPhone 客户端"
date: 2013-11-04 10:40
comments: true
tags:
- rubychina
- nimbus
- opensource
---

最近利用业余时间，用我之前基于nimbus的sina微博框架做了一个Ruby China社区的iPhone客户端。实现社区开放的10来个接口，结合自己的UI审美，制作了一个还算能用的APP，与大家一起学习交流。

注：由于新浪的接口现在阉割的太多，不足以做一个完整的APP，参考[http://www.cocoachina.com/bbs/read.php?tid=165636](http://www.cocoachina.com/bbs/read.php?tid=165636)

因为只用了一周多的时间，也没有单元测试，固然会有很多问题，欢迎fork和pull request，独乐乐，不如众乐乐。

GitHub代码传送门： [https://github.com/jimneylee/JLRubyChina-iPhone](https://github.com/jimneylee/JLRubyChina-iPhone)

![](https://github.com/jimneylee/JLRubyChina-iPhone/raw/master/Resource/Screenshots/home_activity_topics.png)
![](https://github.com/jimneylee/JLRubyChina-iPhone/raw/master/Resource/Screenshots/left_menu_side.png)
![](https://github.com/jimneylee/JLRubyChina-iPhone/raw/master/Resource/Screenshots/node_select.png)
![](https://github.com/jimneylee/JLRubyChina-iPhone/raw/master/Resource/Screenshots/topic_reply.png)

###DONE
* 1、首页热门帖子显示

* 2、帖子详细浏览、帖子回复列表

* 3、帖子关注、收藏、@某人

* 4、回复帖子支持表情选择

* 5、发帖到指定分类，支持markdown语法

* 6、分类节点列表查看

* 7、酷站分组显示

* 8、会员TOP N查看

* 9、我的主页，已发帖子、收藏帖子查看

* 10、Ruby China Wiki

* 11、更多功能包含：清空缓存、更新检测、给我评分、关于APP

* 12、帖子列表支持markdown语法解析显示(仅使用于7.x)

* 13、网络2G/3G/WIFI切换提示


###TODO
* 1、与后台API接口修改确认，参见API Problem文档说明

* 2、发帖添加表情选择

* ~~3、帖子列表支持markdown语法解析显示~~

* ~~4、分类节点做分组与排序~~

* 5、个人主页详细资料

* ~~6、网络2G/3G/WIFI切换提示~~

* 7、发布模式下需屏蔽No Point分类

* 8、增加社交组件分享

* ~~9、经公测稳定，提交AppStore审核，方便大家下载使用~~

* 10、如果需要的话，添加友盟统计

####UPDATE：2013.12.19
---
* 1、发帖支持markdown
* 2、回复支持emoji表情选择

####UPDATE：2013.12.26
---
* 1、帖子正文支持makrdown语法显示，不过正则识别还需进一步完善，欢迎大家讨论交流。

####UPDATE：2013.12.31
---
* 1、帖子正文支持markdown语法，正则匹配还有很多问题，待完善
* 2、显示帖子和回复中图片
* 3、兼容ios6
* 4、添加网络断开/2g3g/wifi切换的时间侦听和提示