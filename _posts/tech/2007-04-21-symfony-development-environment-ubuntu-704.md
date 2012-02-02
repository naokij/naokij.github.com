---
layout: post
title: ubuntu 7.04 下配置symfony开发环境
wordpress_id: 34
date: 2007-04-21 07:35:12.000000000 +08:00
tags: symfony framework php ubuntu
category: tech
---
在ubuntu 7.04下配置symfony非常简单，10分钟内可以搞定。

##安装 Apache2+PHP5+MySQL

	sudo apt-get install apache2 libapache2-mod-php5 php5 php5-gd mysql-server php5-mysql phpmyadmin php5-cli php-pear

安装mod_rewrite

	sudo a2enmod rewrite

常用命令与位置

	sudo /etc/init.d/apache2 restart （重启 apache）
	sudo gedit /etc/php5/apache2/php.ini （配置 php.ini）
	sudo gedit /etc/apache2/apache2.conf （配置 apache2.conf）
	/var/www/ （主目录位置）
	/etc/apache2/sites-enabled (虚拟主机配置文件目录)
	http://localhost/phpmyadmin/  (phpmyadmin)

##安装symfony

	`sudo pear channel-discover pear.symfony-project.com`  
	`sudo pear install symfony/symfony`  

##安装subversion
	sudo apt-get install subversion

##安装编辑器
如果你怕麻烦，可以直接使用ubuntu自带的gedit，这个编辑器跟windows下的EditPlus类似

如果gedit不能满足你，推荐jEdit与Eclipse，安装方法请google

##建立第一个项目Hello World
	cd ~/
	mkdir project/helloworld
	cd project/helloworld
	symfony init-project helloworld
	symfony init-app frontend

##配置虚拟主机
在"系统管理","网络"的"主机"选项组里点"添加",
ip地址填127.0.0.1
别名填helloworld

	sudo gedit  /etc/apache2/sites-enabled/001-helloworld

输入下面的内容

	<VirtualHost *>
	ServerName helloworld
	DocumentRoot "/home/$yourname/project/helloworld/web"
	DirectoryIndex index.php
	Alias /sf /usr/share/php/data/symfony/web/sf
	<Directory "/usr/share/php/data/symfony/web/sf">
	AllowOverride All
	Allow from All
	</Directory>
	<Directory "/home/$yourname/project/helloworld/web">
	AllowOverride All
	Allow from All
	</Directory>
	</VirtualHost>
请把$yourname替换成你的ubuntu用户名

OK,完成
打开Firefoxi, 看这个网址
http://helloworld/
你会看到symfony的成功页面

本文参考了nick的[Ubuntu 7.04 桌面服务器配置](http://www.osxcn.com/ubuntu/ubuntu-feisty-fawn-server.html)
##symfony相关网站
* [symfony官方网站](http://www.symfony-project.com)
* [symfony研究@中国](http://www.symfony-project.cn)

