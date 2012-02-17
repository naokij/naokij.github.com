---
layout: page
title: Jiang Le
---
{% include JB/setup %}

<div id="home">
	<div id="intro">
		{{"你好，我的名字是江乐，目前住在上海。我的工作是php ruby代码还有部门管理。欢迎在[twitter](http://twitter.com/jiangle)或者[微博](http://weibo.com/smartynaoki)上关注我，或者给我[写信](smartynaoki@gmail.com)。" | markdownify}}
	</div>

	<div id="featured">
		<h3>最新文章</h3>

		{% for post in site.posts offset: 0 limit: site.theme.homepage_featured %}
			{% include post_featured.html %}
		{% endfor %}
	</div>

	<div id="posts">
		<h3>近期文章</h3>

		<dl>
		  {% for post in site.posts offset: site.theme.homepage_featured limit: site.theme.homepage_posts %}
				{% include post_summary.html %}
		  {% endfor %}
		</dl>
		<p><a href="/archive.html" class="more">所有文章 »</a></p>
	</div>
</div>
