---
layout: page
title: 日语专栏
description: 日更自己每天学习的一个日语单词( 雾 ), 更新频繁, 内容不多不适合放主页, 单独开一个页面放置
keywords: 日语
comments: true
menu: 日语
permalink: /ja/
---

<section class="container posts-content">
{% assign sorted_categories = site.categories | sort %}
{% for category in sorted_categories %}
{% if category[0]=="日语" %}
<!-- <h3 id="{{ category[0] }}">{{ category | first }}</h3> -->
<ol class="posts-list">
{% for post in category.last %}
<li class="posts-list-item">
<span class="posts-list-meta">{{ post.date | date:"%Y-%m-%d" }}</span>
<a class="posts-list-name" href="{{ site.url }}{{ post.url }}">{{ post.title }}</a>
</li>
{% endfor %}
</ol>
{% endif %}
{% endfor %}
</section>
