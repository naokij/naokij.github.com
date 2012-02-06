---
layout: post
title: 从Wordpress切换到Jekyll
category: tech
tags: jekyll wordpress blog ruby
date: 2012-02-06
---

好久没更新自己的博客，主要是最近迷上了LOL，也比较忙的。直到发现越来越多的人开始用jekyll，让我决定研究下jekyll。最后我决定将这个博客转入jekyll，是下面几个原因:

* 用Mac OS上的编辑器进行本地写作，无需网络就可以预览网站
* 网页可以存放在github
* 不用每次Wordpress发布新版本的时候花10分钟进行升级
* markdown

##安装
jekyll的安装并不像想象的那么简单，不过安装并设置好以后，以后发布博客就很容易了。

我的操作系统是Mac OS X Lion 10.7.2，CentOS, Ubuntu的安装应该跟我的很类似。

### jekyll bootstrap
安装RVM
jekyll在MacOS X Lion 10.7.2自带的ruby 1.8.7无法运行起来，所以要安装最新版的ruby，但是为了不把系统弄乱，我选择安装RVM来管理系统的ruby版本。

	bash -s stable < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)

安装ruby 1.9.3
	rvm install 1.9.3

将ruby 1.9.3设置成默认ruby版本
	rvm --default 1.9.3

安装jekyll
	gem install jekyll

安装jekyll bootstrap, `naokij`是我的github用户名

	git clone https://github.com/plusjade/jekyll-bootstrap.git naokij.github.com
	cd naokij.github.com
	git remote set-url origin git@github.com:naokij/naokij.github.com.git
	git push origin master

###github仓库
在github建立一个仓库，名叫`naokij.github.com`, `naokij`是我的用户名

###启动jekyll
	jekyll --server
###上传到github pages
	git add .
	git commit -m "first commit"
	git push
	
###绑定域名
在`naokij.github.com`目录建立文件`CNAME`

###导入wordpress帖子
由于我的wordpress帖子的固定网址都是直接用的中文，直接转换出来的帖子文件名是乱码，所以我先登陆到Wordpress后台，把每篇帖子的固定网址改成英文。

	ruby -rubygems -e 'require "jekyll/migrators/wordpress"; Jekyll::WordPress.process("database", "user", "pass")'

在浏览器上输入localhost:4000，并没有我导入的帖子，在jekyll --server运行的终端窗口可以看到不少makuru的渲染错误信息。

然后开始了漫长的修改过程，主要注意几点:

####设置rdiscount默认的markdown渲染包
jekyll默认的makuru对中文处理容易出错，在_config.yml里将找到`pygments: true`，在下面添加一行

	markdown: rdiscount
	
####统一换行符
Windows换行符有时候也会造成渲染的错误，所以要把换行符统一成unix的\n

	find ～/project/naokij.github.com/_posts -type f -name "*.md"|xargs perl -pi -e 's/\r$//'
	
####添加标签，类别
这一步不是必须的，也可以修改jekyll的Wordpress导入程序来完成。

###导入comments
申请一个[disqus](http://disqus.com/)帐号，登陆帐号，按照里面的说明，安装Wordpress插件，开始导入Wordpress留言。

导入好以后，在_config.yml里面加上你的disqus帐号。

###pygments代码高亮

	sudo easy_install pygments
	
安装完以后，记得加入pygments的样式。可以参考这个[style.css](https://github.com/mojombo/tpw/tree/master/css/syntax.css)文件，加入到当前模版的css文件里面就可以了。

###完工