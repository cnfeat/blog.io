---
layout: post
title: Make data more human
date: 2015-01-23
categories: blog
tags: [design,ted,visualization]
description: TED观后感，让数据更人性化
---

# 关于作者
![author](http://7xuywf.com1.z0.glb.clouddn.com/dataHuman_Author.jpg)
Jer Thorp  是[The Office for Creative Reasearch](http://o-c-r.org/)的联合创始人纽约大学[ITP项目](http://itp.nyu.edu/itp/)的兼职教授并就职于[New York Times R&D Group](http://nytlabs.com/)，擅长于将人们身边的数据，转化为可视化的意义与故事，从而能更好地了解与掌控自己的数据。

# 演讲概要

如果说该演讲仅仅围绕数据可视化，那我觉得不够精确概括，在一遍又一遍地看完视频后，我倾向于将作者的作品理解为数据人性化。许多人对数字，对公式，对数学不敏感，甚至于排斥，这就给了数据可视化一个生存的空间，亦或者给艺术家一个创造的天地。


但很多时候数据可视化仅仅是为了将数据转化为图标，图形，用各式各样的方法，将数据变得「好看」起来，而忘记了原来的目的，即传达信息。本文主要展示Jer Thorp在TED中讲解的几个案例，从而更好地理解如何更好地将传达数据的信息，甚至传达其中的感情。


一开始觉得讲者的项目介绍没什么规律，写着写着发现，原来是从纽约出发，然后通过社交网络到全球，后来通过NASA跑到了外星系，讲者的野心相当大。

**[TED视频链接](http://www.ted.com/talks/jer_thorp_make_data_more_human)**


# 演讲内容

## [Simulated Economy](http://blog.blprnt.com/blog/blprnt/the-colour-economy-the-gap-between-the-rich-and-the-poor)

![Pixel](http://7xuywf.com1.z0.glb.clouddn.com/dataHuman_pixel.gif)
这是第一个案例，这是作者一个实验性的作品，图中的像素可以看为个体，他们之间互相进行「交易」，互相聚集也相互分离。至于颜色聚集的原理作者并没有细讲。

## [Communism&Terrorism](http://blog.blprnt.com/blog/blprnt/multi-faceted-searching-with-the-nytimes-apis)

![comm&terr](http://7xuywf.com1.z0.glb.clouddn.com/dataHuman_1.jpg
![badExp](http://7xuywf.com1.z0.glb.clouddn.com/logoDesign_badExp.png?	
imageView2/0/w/480/h/480/interlace/0/q/100)
接下来是作者对New York Times中词汇的提取与加工，基本上都是引入了词汇使用数量与时间之间的关系。
首先是Communism&Terrorism（共产主义&恐怖主义）
上方是Communism，下方是Terrorism，图中统计词汇自1981年以来在NY Times的使用频率，浅黄色表示出现在头版的数量，而暗黄色表示。。。。
讲着在主页中给出了获取相关数据的API（应用程序接口），有兴趣可以看看。


## Timepiece graph
![time](http://7xuywf.com1.z0.glb.clouddn.com/dataHuman_2.png?	
imageView2/0/w/480/h/480/interlace/0/q/100)
<!-- ![badExp](http://7xuywf.com1.z0.glb.clouddn.com/logoDesign_badExp.png?	
imageView2/0/w/480/h/480/interlace/0/q/100) -->
接下来的可视化形式是Timepiece graph，顺时针表示一天之内的时间，而颜色代表种类，条形的长度代表数量，这样就可以很直观的看出在哪个时间段，A的数量与B数量的增减情况。其中灰色的表示Despair（绝望）而白色表示（希望），可以看出大概有3次绝望的情绪是超过希望的

## [NYTimes : 365/360](http://blog.blprnt.com/blog/blprnt/7-days-of-source-day-2-nytimes-36536)
![ny]()
该作品为NYTimes 365/360，这组作品的复杂度大大高于之前的，作品时间跨度17年，从1985年到2001年，讲者在制作过程中尝试将每一年的故事提炼出来整合到一张图片里面，理清每年大事记中个人与组织之间的关系，并用线条将其连接。在作者的主页中给出了源代码，使用了技术是Processing。

## [Just Landed](http://blog.blprnt.com/blog/blprnt/just-landed-processing-twitter-metacarta-hidden-data)

在Just Landed中，讲者的数据来源于社交网络，关键词是"Just Landed"「刚降落」，以此来从中模拟出人流传播模型。有意思的是，这个项目的来源是他一位生物信息学博士的朋友，他们讨论了关于H1N1流感病毒的传播，讲者从中得出目前流行病学家正努力寻找一种建立传播病模型的替代方法。这给了讲者启发，他希望通过社交网络的大数据，从中分析出传播模型。
整个模拟的原理比较简单，Just Landed 后面为目的地，而用户资料有所在地为出发地，发状态的时间为传播结束时间，实现方法同样在讲者博客中有详细的介绍。

## [Good morning](http://blog.blprnt.com/blog/blprnt/goodmorning)

同样是社交网路数据，关键词是"Good Morning"，作者选取某一天，爬下了11,000句Good Morning的Tweets（状态），在数据可视化中存在这么几个变量，地理坐标，发Tweets的时间和数量，而时间体现在颜色上，绿色表示很早，橙色表示大约早上9点，红色则是根本不再早上，黑色表示奇怪的时间。（发现中国是空白一片，一定是因为中国是非英语使用国家：），这个项目比较有意思，经过了好几个版本的迭代，讲者的博客同样进行了详细的讲解。

## Kepler Project

该作品来源于美国国家航空局的项目Kepler mission--用来发现其他恒星的类地行星，比较科普的介绍方法参见[这里](http://zh.wikipedia.org/wiki/%E5%85%8B%E5%8D%9C%E5%8B%92%E5%A4%AA%E7%A9%BA%E6%9C%9B%E9%81%A0%E9%8F%A1)，这个作品讲者的介绍较少，也是有源代码实现，有兴趣的人可以试试。

## [Cascade](http://nytlabs.com/projects/cascade.html)

Cascade 是用以分析社交网络信息传播途径结构的模型，也叫Cascade结构，由下图可以看出，该结构模拟信息传播扩散途径，一件事情发生后由互联网通过一个节点，向其他节点扩散。
而真实世界下的扩散结构图更为复杂，一个信息的扩散随时间流逝形成网络，因此存在几个变量，在图【】中，Y轴方向表示分散度（Degrees of Separation），X轴方向表示时间。而在图【】中，可以清晰地看出信息的扩散路径（threads of conversation
），而在图【】中，则是Cascade结构图的三维模型。


## [9/11纪念碑](http://blog.blprnt.com/blog/blprnt/all-the-names)

该项目是为了纪念911逝者，其与众不同之处在于刻在纪念碑上的人名不是以一种机械的方式排列，比如字母顺序，笔画顺序或者被限制在表格中。而是从人这个个体开始，挖掘他们之间的人际关系，同事，家人，朋友，根据这些关系，来决定名字的排列顺序，用以缅怀逝者。那么如何找出这些关系，也就是如下这个工具的功能。
目前该工具就2900个名字，从中找出了1400个相关关系。讲者简单介绍了该工具：首先是自动产生并罗列出所有的关系，然后根据挑选出一些关系配合所需要叙述的故事来设计纪念碑。


## [Open Path](http://blog.blprnt.com/blog/blprnt/your-device-your-data-how-to-save-your-iphone-location-data-and-help-researchers-make-the-world-a-better-place)

在此，讲者把视角转入了手机的地理位置记录数据上。Open Path通过接受手机的地理位置数据，让手机用户自身可以知道自己之前的行走路径。讲者介绍了自己曾经的参加，曾经的约会，以宣传一个概念，就是让用户自身能知道自己的小历史「所在，所做，所见」。这样就将这些碎片信息拼接成了自己的历史，而这正是数据人文化的一种体现吧。

设备中储存的数据，经过加工与处理，就可以体现人文关怀，甚至记录了我们自己曾经的历史
最开始，我们整理数据，只是仅仅希望能更好地理解数据的意义，
但是深入处理了之后，结合其他的一些数据，将之与人文环境结合，
那么数据就好似获得了生命，有了感情。
那我们开始更重视技术背后的意义，甚至我们了解到它们设计的隐私问题。
因为数据并不是数字，它与我们现实的生活相互连接。
正如讲者所说：

> They carry weight  .




