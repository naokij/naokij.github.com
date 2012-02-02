---
layout: post
title: symfony 1.0.1版发布
wordpress_id: 20
date: 2007-03-20 17:23:23.000000000 +08:00
tags: symfony framework php
category: tech
---
symfony 1.0.0版发布一个月之后，symfony 1.0.1版发布了。这个版本没有新功能，修改了一些bug.

* r3624: fixed security.yml case sensitivity
* r3599: fixed sfYaml::load() not returning correct values
* r3598: removed unneeded usage of JavaScript helpers in the web debug toolbar
* r3597: fixed sfConsoleRequest::initialize() signature
* r3541: fixed typo in the cache classes when logging

主要的bug修复是security.yml配置文件还有动作大小写问题。如果你的程序包受保护的模块，推荐升级到1.0.1。
pear方式安装请使用pear upgrade symfony/symfony 升级。升级完成之后注意清空缓存(symfony clear-cache)。
