---
layout: project
title: "Portfolio"
subtitle: "媒体技术与艺术君，专注于产品设计，交互设计以及界面设计"
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