---
layout: post
title: 部分网友上不了fmfans的解决方法
wordpress_id: 60
wordpress_url: http://www.jiangle.name/archives/60
date: 2007-10-19 15:04:02.000000000 +08:00
tags: fmfans dns
category: tech
---
这两天不少人反映上不了论坛，原因是论坛dns服务器出现故障，现在公布临时解决办法

故障期间，请用fmfans.cpgl.net这个网址访问论坛，如果这个网址也不行，请用下面的两个办法中的任意一种解决

另外,请大家互相转告解决办法!

##方法1
下载附件，解压得到一个bat文件，双击这个bat文件，关闭浏览器，然后再重开浏览器

[hosts文件补丁](http://i.jiangle.name/wp-content/uploads/2007/10/20071019_c0b60b55a80338fb0b2dkhojcspb7waa.zip)


##方法2
用记事本打开c:\windows\system32\drivers\etc\hosts这个文件，在最后增加一行内容

	219.234.95.186 fmfans.cpgl.net

关闭浏览器，然后再重开浏览器