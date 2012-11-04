---
layout: post
title: nginx spider过滤脚本
date: 2012-11-03
category: tech
tags: nginx spider bash
---
前段时间，FMFans更换域名以后，访问速度突然变的非常慢。查了nginx日志之后，发现是大量的spider造成的。于是找了个自动根据nginx日志过滤spider的脚本，分享如下:

<script src="https://gist.github.com/3943499.js?file=spider-killer.sh"></script>