---
layout: post
title: rsyslogd + graylog2 日志管理方案(1)
date: 2012-02-17
category: tech
tags: graylog2 rsyslog ryslog
---
##为什么还要加上rsyslogd?
graylog2web界面相比rsyslogd的cli界面相比好上一百倍，而且支持很多新的协议。为什么要加上rsyslogd呢？

syslog是一个很古老的协议，很多设备导出的日志不标准，直接用graylog2会有各种各样的问题。rsyslog在这方面比较强悍，所以把rsyslog放在前端，对各设备的syslog处理之后再导出给graylog2。

##相关系统&软件
* CentOS 6.2
* rsyslog 4.6.2
* graylog 0.9.6

##安装rsyslogd
我的系统是CentOS 6.2，已经自带了rsyslog，不用再另外安装了。但是需要做一些额外的设置。

由于我仅仅用rsyslogd转发设备发送的日志到graylog2，所以不需要服务器本地的日志。但是CentOS 6.2自带的rsyslogd版本过低，不支持转发的时候绑定规则，所以我的办法是运行第二个rsyslogd进程，专门用来转发。

###启动脚本

	vi /etc/init.d/rsyslogforwarder
	
输入下面的内容

<script src="https://gist.github.com/1846479.js"> </script>

修改权限

	chmod a+x /etc/init.d/rsyslogforwarder


###配置文件

	vi /etc/rsyslogforwarder.conf
	
<script src="https://gist.github.com/1846489.js?file=rsyslogforwarder.conf"></script>

其中`*.* @@127.0.0.1:1514`这一行指定转发的目的地，本机的1514 TCP端口。稍后会将graylog2安装在这个端口。

###启动

`/etc/init.d/rsyslogforwarder start`

###设置自动启动
`chkconfig rsyslogforwarder on`

##参考资料
* [HOWTO: install graylog2 on CentOS 5 with RVM + Passenger](http://joemiller.me/2011/04/13/howto-install-graylog2-on-centos-5-with-rvm-passenger/)
* [Rsyslogd with MySQL on RHEL / CentOS 6 HOWTO](http://www.standalone-sysadmin.com/blog/2011/09/rsyslogd-with-mysql-on-rhel-centos-6-howto/)
* [Upgrading Graylog2 from 0.9.5p2 to 0.9.6 with data migration](http://andreas-lehr.com/blog/archives/556-upgrading-graylog2-from-0-9-5p2-to-0-9-6-with-data-migration.html)
* [Forwarding from rsyslog](https://github.com/Graylog2/graylog2-server/wiki/Forwarding-from-rsyslog)