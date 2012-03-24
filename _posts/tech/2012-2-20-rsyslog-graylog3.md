---
layout: post
title: rsyslogd + graylog2 日志管理方案(3) backlist
date: 2012-02-20
category: tech
tags: graylog2 rsyslog ryslog blacklist
---
graylog2-web-interface里的blacklist使用的是正则表达式，虽然写正则表达式并不是很难，但是这个graylog2功能很奇怪，并不是总能生效。所以我写了一个脚本，将graylog2的blacklist导出，写 入到rsyslog的配置文件，让rsyslog去做过滤。

##graylog2-blacklist-2-rsyslog.rb

<script src="https://gist.github.com/1868537.js?file=graylog2-blacklist-2-rsyslog.rb"></script>

记得修改'grayloguser','grayloguser-mongo-passwd'

在/etc/rsyslogforwarder里增加一行

	$IncludeConfig /etc/rsyslog_disgarding.conf
	
重启rsyslog

	/etc/init.d/rsyslogforwarer restart
	
将graylog2-blacklist-2-rsyslog.rb加入cron

	crontab -e
	*/5 * * * * /usr/local/rvm/bin/ruby-1.9.3-p0 /opt/graylog2-rsyslog-blacklist/graylog2-blacklist-2-rsyslog.rb