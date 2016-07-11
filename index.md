---
layout: page
title: Welcom!
tagline: 技术、生活
---
{% include JB/setup %}

## 博客文章

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

## 说明

本博客为个人博客。


