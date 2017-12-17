---
layout: post
title: 搭建github博客（jekyll）
date: 2017-12-17
categories: blog
tags: [github,jekyll]
description: 本篇博客是用github搭建完的首篇，配合jekyll工具。
---
#前言
- 花了两个半天的时间，搭建了自己的github博客，走了不少冤枉路，下面记录一下搭建流程，给需要的人一个快速上手的引导。
- 以下说的repository即指仓库（用github来同步项目/工程文件的话，repository也说成项目）
- 以下说的username即为注册的用户名

#一. 使用jekyll搭建github博客的准备工作
1.安装Node.js（https://nodejs.org/en/）和Git(https://git-scm.com/)
    
2.安装github desktop，工具可以在官网上下载。这个GUI大大简化了敲代码的流程，属于新手上手的好工具。

3.安装jekyll，工具可以官网上下载。

4.注册并登录GitHub
访问：http://www.GitHub.com/
注册你的 username 和邮箱。邮箱十分重要，GitHub 上很多通知都是通过邮箱发送。

4.配置 SSH keys。该教程网上很多，不再赘述。

注：此处不用独立域名，个人感觉github网址的访问形式足够了，暂不介绍独立域名的申请和配置方式。

#二.使用 GitHub Pages 建立博客
##Fork已经设置好的仓库
	1. 登录github
	2. 笔者使用的是 cnfeat/blog.io 
	另外jekyll的wiki上以及jekyll的模板网站上（https://jekyllthemes.io/）有很多。
	此处使用 cnfeat/blog.io ，打开https://github.com/cnfeat/blog.io/tree/master。
	3. 点击右上角 fork ，稍等片刻就会在你的仓库里有fork过来的模板。
	4. 在setting下把cnfeat改成自己的用户名，此时“你的用户名/blog.io”这样的repository就生成了。
	5. 访问https://github.com/你的用户名/blog.io 能够访问的话就代表fork成功了。

##使用github desktop来clone博客
	1. 打开安装的github desktop。首次打开需要登录。
	2. 登陆后选择clone，选择“你的用户名/blog.io”，然后选择clone到本地的哪个目录，等待clone结束。
	3. 打开该目录。

##修改成为自己的博客
	修订清单如下，文档内有详细注释，可按注释逐个修订

	- 博客名字及作者信息：_config.yml  注：注意修改的时候不要改动格式，否则无法同步修改内容

	- 个人介绍页面：about.md
	
	- 代表作页面：milestone.md
	
	- 文章模板：blog.io/_posts/2015-03-02-how-to-write.md  注：按照文章模板编写新的博文

##使用github desktop来更新博客
	1. 打开github desktop，点击相应的repository可以看到修改的内容
	2. 填写summary，点击commit to master，将本地修改的内容同步到本地仓库。
	注：summary一定要填写，否则无法commit to master
	3. 点击右上角的位置Sync或者是pull origin（版本不同），把本地修改后的仓库内容提交到github网站
	4. 打开https:你的用户名.github.io即可看到更新后的博客。

#三. 遗留问题
	1. 目前还不了解整个的运行流程
	2. 不会在本地进行预览














