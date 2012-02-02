---
layout: post
title: 升级到WordPress 2.7
wordpress_id: 78
wordpress_url: http://www.jiangle.name/?p=78
date: 2008-12-23 23:20:07.000000000 +08:00
tags: wordpress tool
category: tech
---
刚刚upgrade好，前台全变乱码了。折腾了半天，终于弄好了：

修改wp-config.php

define('DB_COLLATE', 'ascii-bin');

一般人都用的utf8-general-ci，我的只有ascii-bin才能正常，搞不懂。
