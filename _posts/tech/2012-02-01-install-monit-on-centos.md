---
layout: post
title: 在CentOS服务器上安装Monit
tags: server centos monit
date: 2012-02-01
category: tech
---
monit是一个开源的系统服务监控软件，它可以在任何服务挂掉的时候自动重启这个服务。

##下载安装

	wget http://mmonit.com/monit/dist/monit-5.3.2.tar.gz
	tar xzvf monit-5.3.2.tar.gz
	cd monit-5.3.2
	./configure
	make
	make install
	
复制控制文件到/etc

	cp monitrc /etc/

编辑/etc/monitrc

	vi /etc/monitrc

按`shift+g`跳到文件最后，取消`include /etc/monit.d/*`这行的注释，

查找`allow @monit`和`allow @users readonly`注释掉这两行

搜索`use address`, 把后面的`localhost`改成服务器ip地址

搜索`allow localhost`，按`o`，在下面添加`allow 64.78.160.0/24`，允许这些ip地址访问

保存。

修改/etc/monitrc权限

	chmod 0700 /etc/monitrc

##监控php-fastcgi
现在我们要监控php-fastcgi, 建立/etc/monit.d/php文件

	mkdir /etc/monit.d
	vi /etc/monit.d/php

增加下面的内容

	check process php-cgi with pidfile /usr/local/webserver/php/logs/php-fpm.pid
	group php
	start program = "/usr/local/webserver/php/sbin/php-fpm start"
	stop program = "/usr/local/webserver/php/sbin/php-fpm stop"
	
	if failed host 127.0.0.1 port 9000 then restart
	if 3 restarts within 5 cycles then timeout

##启动
	/usr/local/bin/monit -d 60 -v -c /etc/monitrc  -p /var/run/monit.pid -l /var/log/monit.log

打开浏览器访问`xxx.xxx.xxx.xxx:2812`，xxx.xxx.xxx.xxx是服务器的ip地址。

##自动启动
修改/etc/rc.local

	vi /etc/rc.local

在最后添加

	/usr/local/bin/monit -d 60 -v -c /etc/monitrc  -p /var/run/monit.pid -l /var/log/monit.log
	
##参考资料
1. [用monit监控系统关键进程](http://feilong.me/2011/02/monitor-core-processes-with-monit)
2. [Installing Monit On Linux CentOS Server](http://blog.hostonnet.com/installing-monit-on-linux-centos-server)
3. [Monit Homepage](http://mmonit.com/monit/)


