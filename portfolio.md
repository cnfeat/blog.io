---
layout: page
title: "Portfolio"
description: "你看到的，是历年的文章"
header-img: "img/orange.jpg"
---


<ul class="listing">
{% for post in site.posts %}
{% if post.type == "project" %}
  {% capture y %}{{post.date | date:"%Y"}}{% endcapture %}
  {% if year != y %}
    {% assign year = y %}
    <li class="listing-seperator">{{ y }}</li>
  {% endif %}
  <li class="listing-item">
    <time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
    <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
  </li>
{% endif %}
{% endfor %}
</ul>