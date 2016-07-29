---
layout: project
title: "Portfolio"
subtitle: "林龙飞"
description: "媒体技术与艺术君，专注于产品设计，交互设计以及界面设计"
header-img: "img/orange.jpg"
---


<!-- <ul class="listing">
{% for post in site.posts %}
{% if post.type == "project" %}
  {% capture y %}{{post.date | date:"%Y"}}{% endcapture %}

  <li class="listing-item">
    <time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
    <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
  </li>
{% endif %}
{% endfor %}
</ul> -->

{% for post in site.posts %}
{% if post.type == "project" %}
<div class = "main">
	<ul class= "cbp_tmtimeline">
		<li>
			<div class = "cbp_tmlabel">
				<h2 id = "boxoffice">{{post.title}}</h2>
				<a>{{post.date}}</a>
				<img src="{{post.imgsrc}}">
				<ul>
					<li>
						{{post.description}}
					</li>
					<li class = "skill">
						{% for skill in post.skills %}
						<span><b>{{skill}}</b></span>
						{% endfor%}
						<span class = "link">
							<a target="_blank" href="{{post.url}}">查看</a>
						</span>
					</li>
				</ul>
			</div>
		</li>	
	</ul>
</div>

{% endif %}
{% endfor %}