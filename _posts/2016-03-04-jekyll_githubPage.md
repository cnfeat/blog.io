---
title: Jekyll在github上构建免费的个人博客
tags: github,jekyll
grammar_cjkRuby: true
---
**jekyll介绍**
Jekyll是一个静态站点生成器，它会根据网页源码生成静态文件。它提供了模板、变量、插件等功能，可以用来生成整个网站。

Jekyll生成的站点，可以直接发布到github上面，这样我们就有了一个免费的，无限流量的，有人维护的属于我们的自己的web网站。  

**安装jekyll**
window环境下比较快捷的方法是到 http://www.madhur.co.in/blog/2013/07/20/buildportablejekyll.html 直接下载并解压到任意路径即可。在管理员CMD下进入解压的portable Jekyll所对应的路径运行setpath.cmd即可。喜欢一步一步折腾安装的同学，可参考： http://www.madhur.co.in/blog/2011/09/01/runningjekyllwindows.html 

**搭建github个人博客**
一个快速的方式是直接到我的github库将博客模版fork下来：
https://github.com/Myranda/Myranda.github.io，在setting里把myranda改成你想要的名字，并点击“Launch  automatic page generator”按钮，选择模版。然后clone项目到你自己的机器上来，想要在本地查看效果
 

       cd USERNAME.github.com
       jekyll serve

这样你就可以在本地修改并查看效果了。
另外，博客文章存放在_posts文件夹里，命名规则为(年-月-日-文章标题.md),md格式文件其实是markdown格式的文档，若不想记markdown语法的童鞋可使用markdown编辑器，如小书匠<a>http://markdown.xiaoshujiang.com/</a>编辑好文章后再复制粘帖进项目。

修改完成后提交代码，提交代码指令请看<a>http://myranda.github.io/2016/03/03/github_command/</a>
提交完成后访问
http://username.github.com/就可以看到Blog已经生成了（将username换成你的用户名）。