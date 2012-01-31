---
layout: page
title: Jiang Le
---
{% include JB/setup %}

##{{site.posts.first.title}}

{{site.posts.first.content}}
    

<hr />

## 最近发布的文章
<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>


