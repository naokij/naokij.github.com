---
layout: page
title: Jiang Le
---
{% include JB/setup %}

欢迎访问我的个人网站，欢迎在[Twitter](http://twitter.com/jiangle)或者[微博](http://weibo.com/smartynaoki)上关注我，或者给我写信(smartynaoki@gmail.com)。


## 最近发布的文章
<ul class="posts">
  {% for post in site.posts % limit:10 %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>


