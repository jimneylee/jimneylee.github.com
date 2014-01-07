---
author: jimneylee
layout: post
title: "制作一个APNS的推送证书"
date: 2012-04-18 10:35:23
comments: true
tags:
- iphone
- apns
- php
- pem
---
1、为应用创建独立的app ID和开发、发布的profile文件。同时开启push功能，完成后双击安装。

2、从钥匙串访问(Keychain Access)中，`我的证书`对应APP的开发或发布证书，分别导出cert和key，命名为`apns-dev-cert.p12`和`apns-dev-key.p12`。

3、终端命令行运行下面的`opnessl`命令，即可生存所需的`pem`文件

{% highlight bash %}
$ openssl pkcs12 -clcerts -nokeys -out apns-dev-cert.pem -in apns-dev-cert.p12
$ openssl pkcs12 -nocerts -out apns-dev-key.pem -in apns-dev-key.p12
$ openssl rsa -in apns-dev-key.pem -out apns-dev-key-noenc.pem
$ cat apns-dev-cert.pem apns-dev-key-noenc.pem > apns-dev.pem
{% endhighlight %}

写的很粗略，这位[老外同学](http://quickblox.com/developers/How_to_create_APNS_certificates)写的很详细，步骤备份此处，有不理解的同学可以留言！

**PS:**今天发现诡异的现象，之前制作development证书时，`Enter PEM pass phrase:`，密码我输入４个１可以的，今天制作release的pem证书，输入４个１报错：
`unable to load Private Key 140735097526716:error:0906D06C:PEM
routines:PEM_read_bio:no start line:pem_lib.c:696:Expecting: ANY PRIVATE KEY`

然后，我花费了很长时间，重新做cert和key原始的p12证书，反复核对，信息也无误。因为之前我用同样的步骤已经做成一个app的push证书。

所以我继续尝试，最后我再上面键入密码时候，输入６个１，就没问题了。我勒个去，看来是因为我对`openssl`命令还不甚了解吧。

希望对看到的同学有帮助，也可能是我遇到的特例，不过成功了，心里还是有点安慰。　:)