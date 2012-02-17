---
layout: post
title: rsyslogd + graylog2 日志管理方案(2)
date: 2012-02-17
category: tech
tags: graylog2 rsyslog ryslog
---
##graylog2安装
graylog2 是一个开源的日志存储系统，它由下面几部分组成：

* 一个由Java写的server，接受TCP、UDP及AMQP的syslog日志信息 用来存储日志内容的MongoDB服务器 * 一个web管理界面使用ruby编写，基于Rails框架，让你可以可视化地管理你的日志

主要特性

* 通过 TCP/UDP 接收syslog信息
* 基于 MongoDB 的快速后端存储
* GELF (Graylog Extended Log Format)
* 黑名单
* 过滤器
* 统计

###相关汇总
* 在`/opt/graylog2-server`目录安装graylog2-server
* 在`/opt/graylog2-web-interface`目录安装graylog2-web-interface in 
* graylog2-web-interface以普通用户`graylog`身份运行
* 通过RVM安装Ruby 1.9.3和Rails 3.x环境
* 通过Passenger-standalone，在http://127.0.0.1:3001/运行graylog2-web-interface
* Apache监听tcp/80端口，通过mod_proxy转发3001端口的graylog2-web-interface

###安装MongoDB
通过 EPEL yum repo 安装mongodb 
	# yum install mongodb mongodb-server
	# mkdir /var/lib/mongodb
	# chown -R mongodb:mongodb /var/lib/mongodb/ 	 
设置开机自动启动mongod 
	# chkconfig mongod on 	 
开启mongod(日志: /var/log/mongodb/mongodb.log) 
	# service mongod start 	 
运行`mongo`，在`admin` collection里创建一个`admin`用户，新建`graylog2` collection还有`graylog`用户。
	# mongo
		use admin
		db.addUser('admin', 'admin-mongo-passwd')
		db.auth('admin', 'admin-mongo-passwd')
		use graylog2
		db.addUser('grayloguser', 'grayloguser-mongo-passwd')

###安装elasticsearch
首先是下载
cd /opt
wget https://github.com/downloads/elasticsearch/elasticsearch/elasticsearch-0.18.7.tar.gz tar xzfv elasticsearch-0.18.7.tar.gz

基本设置
	vi /opt/elasticsearch/config/elasticsearch.yml
`network.host:` ，设置成`127.0.0.1`
`path.logs`，设置成`/var/log/elasticsearch` 
`path.data`，设置成`/var/data/elasticsearch`
`cluster.name`，设置成`graylog2`


下载elasticsearch-servicewrapper (tanuki-wrapper) 

	cd /opt/elasticsearch/bin/
	wget https://github.com/elasticsearch/elasticsearch-servicewrapper/zipball/master 	mv master elasticsearch-servicewrapper.zip && unzip elasticsearch-servicewrapper.zip 	mv elasticsearch-elasticsearch-servicewrapper-*/* . && rm -rf elasticsearch-elasticsearch-servicewrapper-*
	
启动elasticsearch
	/opt/elasticsearch/bin/service/elasticsearch start
	
查看是否启动成功

	tail /var/log/elasticsearch/graylog2.log
	正常情况会显示文件内容

设置自动启动
	vi /etc/rc.local
	加上一行
	/opt/elasticsearch/bin/service/elasticsearch start

###安装graylog2-server
下载最新版graylog2-server压缩包: <https://github.com/Graylog2/graylog2-server/downloads>

	mkdir /opt
	cd /opt
	wget https://github.com/downloads/Graylog2/graylog2-server/graylog2-server-0.9.6.tar.gz

安装到/opt/graylog2-server

	tar xzvf graylog2-server-0.9.6.tar.gz
	ln -s graylog2-server-0.9.6 graylog2-server

修改配置文件`/etc/graylog2.conf`
		cp /opt/graylog2-server/graylog2.conf.example /etc/graylog2.conf
		编辑/etc/graylog2.conf 设置monogdb_user, mongodb_password

启动脚本`/etc/init.d/graylog2-server`
<script src="https://gist.github.com/1851983.js?file=graylog2-server"></script>

日志切割配置`/etc/logrotate.d/graylog2-server`
<script src="https://gist.github.com/1851995.js?file=graylog2-server"></script>

注册graylog2-server启动脚本
	chmod +x /etc/init.d/graylog2-server
	chkconfig --add graylog2-server
	chkconfig graylog2-server on

启动graylog2-server
	service graylog2-server start
	ps aux | grep graylog2
		root     23197  2.3  0.1 1196204 18428 pts/0   Sl   12:37   0:00 java -jar ../graylog2-server.jar

##参考资料
* [HOWTO: install graylog2 on CentOS 5 with RVM + Passenger](http://joemiller.me/2011/04/13/howto-install-graylog2-on-centos-5-with-rvm-passenger/)
* [Rsyslogd with MySQL on RHEL / CentOS 6 HOWTO](http://www.standalone-sysadmin.com/blog/2011/09/rsyslogd-with-mysql-on-rhel-centos-6-howto/)
* [Upgrading Graylog2 from 0.9.5p2 to 0.9.6 with data migration](http://andreas-lehr.com/blog/archives/556-upgrading-graylog2-from-0-9-5p2-to-0-9-6-with-data-migration.html)
* [Forwarding from rsyslog](https://github.com/Graylog2/graylog2-server/wiki/Forwarding-from-rsyslog)