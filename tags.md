---
layout: page
title: "Tags"
description: "这里是，文章的一些关键词"  
header-img: "img/semantic.jpg"  
---

### 本页使用方法

1. 在下面选一个你喜欢的词
2. 点击该词
3. 相关的文章会「唰」地一声跳到页面顶端
4. 马上试试？

### 基因列表



<!-- Main Content -->
<div class="container">
	<div class="row">
		<div class="col-lg-8 col-md-10">
            <!-- 标签云 -->
			<div id='tag_cloud' class="tags">
				{% for tag in site.tags %}
				<a href="#{{ tag[0] }}" title="{{ tag[0] }}" rel="{{ tag[1].size }}">{{ tag[0] }}</a>
				{% endfor %}
			</div>
            <!-- 标签列表 -->
			{% for tag in site.tags %}
			<div class="one-tag-list">
			  	<span class="fa fa-tag listing-seperator" id="{{ tag[0] }}">
                    <span class="tag-text">{{ tag[0] }}</span>
                </span>
				{% for post in tag[1] %}
				  <!-- <li class="listing-item">
				  <time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
				  <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
				  </li> -->
				 <div class="post-preview">
				    <a href="{{ post.url | prepend: site.baseurl }}">
				        <h2 class="post-title">
                            {{ post.title }}
				        </h2>
				        {% if post.subtitle %}
				        <h3 class="post-subtitle">
				            {{ post.subtitle }}
				        </h3>
				        {% endif %}
				    </a>
				    <!-- <p class="post-meta">{{ post.date | date:"%Y-%m-%d" }}</p> -->
				</div>
				<hr>
				{% endfor %}
			</div>
			{% endfor %}

		</div>
	</div>
</div>
