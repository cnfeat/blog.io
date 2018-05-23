---
layout: post
title: MarkDown学习入门
date: 2018-5-22
categories: 入门
tags: [MarkDown,入门,初学者]
header-img: "img/posts.jpg"
description: MarkDown入门。
---

所有内容均为测试MarkDown,内容来自于 https://blog.csdn.net/sun8112133/article/details/79871702,特此声明
<h2 id="二基本用法-1"><em>二、基本用法</em></h2>
1、单个回车，视为空格   
2、连续回车，才能分段   
3、行尾加两个空格，就可以段内换行  
4、注释可使用HTML的注释   
5、也可以使用HTML标签  

<h2 id="三语法规则-1"><em>三、语法规则</em></h2>
<h4 id="1标题">1、标题</h4>
<pre><code>#　H1标题
##　H2标题
###　H3标题
####　H4标题
#####　H5标题
######　H6标题
</code></pre>
<h4 id="2列表">2、列表</h4>
<h5 id="1无序列表">　　1）无序列表（*,+,-）</h5>
<pre><code>* 列表1
* 列表2
+ 列表3
+ 列表4
- 列表5
- 列表6
</code></pre>
<h6 id="嵌套">　　<strong><em>嵌套：</em></strong></h6>
<pre><code>* 列表1
    * 子列表1
    * 子列表2
* 列表2
</code></pre>
<h5 id="2有序列表数字点号">　　2）有序列表（数字+点号）</h5>
<pre><code>1. 文本
2. 音乐
    1. 童话
    2. 列了都要爱
    3. 天下
3. 电影
</code></pre>
<h4 id="3文字格式">3、文字格式</h4>
<h5 id="1粗体">　　1）粗体</h5>
<pre><code>** 粗体 **
__ 粗体 __
</code></pre>
<h5 id="2斜体">　　2）斜体</h5>
<pre><code>* 斜体 *
_ 斜体 _
</code></pre>
<h5 id="3粗体斜体">　　3）粗体+斜体</h5>
<pre><code>*** 粗斜体 ***
___ 粗斜体 ___
</code></pre>
<h5 id="4删除markdown-pad2-暂不支持">　　4）删除（MarkDown Pad2 暂不支持）</h5>
<pre><code>~~ 斜体 ~~
</code></pre>
<h4 id="4链接">4、链接</h4>
<h5 id="1直接设置行内形式">　　1）直接设置（行内形式）</h5>
<p>　　<strong>语法：[链接名称](链接地址 “链接title”)</strong></p>
<pre><code>[百度](http://www.baidu.com "百度一下，你就知道")
</code></pre>
<h5 id="2间接设置参考形式">　　2）间接设置（参考形式）</h5>

<p>　　<strong>语法：[链接名称][标记]</strong>
　　　　　<strong>[标记]: 链接地址 “链接title”</strong></p>

<pre><code>[百度][1]
[1]: http://www.baidu.com "百度一下，你就知道"
</code></pre>
<h5 id="3隐式设置">　　3）隐式设置</h5>
<p>　　<strong>语法：[链接名称][]</strong>
　　　　　<strong>[链接名称]: 链接地址 “链接title”</strong></p>
<pre><code>[百度][]
[百度]: http://www.baidu.com "百度一下，你就知道"
</code></pre>
<h4 id="5图片">5、图片</h4>
<h5 id="1直接设置行内形式-1">　　1）直接设置（行内形式）</h5>
<p>　　<strong>语法：![替代文本](链接地址 “链接title”)</strong></p>
<pre><code>![百度](https://www.baidu.com/img/bd_logo1.png "百度一下，你就知道")
</code></pre>
<h5 id="2间接设置参考形式-1">　　2）间接设置（参考形式）</h5>
<p>　　<strong>语法：[替代文本][标记]</strong>
　　　　　<strong>[标记]: 链接地址 “链接title”</strong></p>
<pre><code>![百度][1]
[1]: https://www.baidu.com/img/bd_logo1.png "百度一下，你就知道"
</code></pre>
<h4 id="6引用">6、引用</h4>
<pre><code>>; 一级引用
>;>; 二级引用
>;>;>; 三级引用
</code></pre>

<h5 id="1引用换行">　　1）引用换行：</h5>
<p>　　末尾加两个空格。</p>
<h5 id="2引用内包含其他语法">　　2）引用内包含其他语法：</h5>
<p>　　<strong>如：</strong>标题、列表、代码块 </p>
<p> <strong>注：一定要写在引开头处</strong></p>
<h4 id="7水平分隔线">7、水平分隔线</h4>
<pre><code>---
___
***
</code></pre>
<h4 id="8代码块">8、代码块</h4>
<h5 id="1代码句">　　1）代码句</h5>
<p>　　用反引号（`），就是英文状态下的波浪线。</p>
<pre><code>`我是代码句`
</code></pre>
<h5 id="2代码段">　　2）代码段</h5>
<p>　　4个空格（或Tab缩进）定义代码块。</p>
<pre><code>我是代码段1
我是代码段2
我是代码段3
</code></pre>

<h5 id="3用三个以上的反引号定义段开始和结束">　　3）用三个以上的反引号定义段开始和结束</h5>
<pre><code>``` java
public class Test {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```
</code></pre>

<h4 id="9表格markdown-pad2-暂不支持表格">9、表格（MarkDown Pad2 暂不支持表格）</h4>

<strong>关于冒号（:）</strong> 
<strong>左边：</strong>以下内容左对齐 
<strong>右边：</strong>以下内容右对齐 
<strong>两边：</strong>以下内容居中对齐

<pre><code>| 项目      |    价格 | 数量  |
| :-------- | --------:| :--: |
| Computer  | 1600 元 |  5   |
| Phone     |   12 元 |  12  |
| Pipe      |    1 元 | 234  |
</code></pre>


<h4 id="10文档目录markdown-pad2-暂不支持">10、文档目录（MarkDown Pad2 暂不支持）</h4>
只需在你想要放入目录结构的位置写入 <code>[TOC]</code> 即可。  


<h4 id="11转义字符">11、转义字符</h4>
<p>有的时间在使用MarkDown进行排版时，可以很方便的快速进行排版，但是有时还需要使用一些MarkDown语法中特殊的符号（比如：*，-，+这些），该怎么办呢？我们可以借助转义字符达到我们想要的效果。</p>
<pre><code>\\　反斜杠
\`　反引号
\*　星号
\_　下划线
\+　加号
\-　减号
\#　井号
\.　句号
\~　感叹号
</code></pre>
<h4 id="12uml-图">12、UML 图</h4>



<h5 id="1渲染序列图">　　1）渲染序列图：</h5>

<pre><code>```sequence
小异常->;大异常: 嘿，老大, 看完博客评论了没?
Note right of 大异常: 大异常愣了一下，说：
大异常-->;小异常: 呀，差点忘了，马上评论
```
</code></pre>


<h5 id="2渲染流程图">　　2）渲染流程图：</h5>

<pre><code>``` flow
st=>;start: 开始
e=>;end: 结束
com=>;operation: 开始评论
cond=>;condition: 确认评论？

st->;com->;cond
cond(yes)->;e
cond(no)->;com
```
</code></pre>

