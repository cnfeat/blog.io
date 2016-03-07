---
title: 用firebug调试ie
tags: firebug,
grammar_cjkRuby: true
---


做网页的人都喜欢用firefox，因为firefox有个firebug调试工具，非常符合前端人员的口味。但是为了页面兼容，在IE6+系列调试时就非常让人头疼，在不知道IE也可以使用firebug之前，都是使用IE Developer Toolbar来调试，调试起来让人一头雾水。后来发现IE也可以使用firebug，感觉还不错，总结给大家分享一下：
1、桌面点击右键，新建个文本文档，把下面这段代码粘贴在文档中，把文档重命名为firebug.htm，用ie浏览器打开此htm文件，在链接firebug-to-ie处点击右键，添加到收藏夹。

	<a href="javascript:(
	function(){
	var d=document, 
	s=d.getElementById('firebug-lite');
	if(s!=null)  return;
	s=d.createElement('script');
	s.type='text/javascript';
	s.src='https://getfirebug.com/firebug-lite.js';d.body.appendChild(s);})();void(0);">firebug-to-ie</a> 

打开想调试的页面，打开收藏夹，点击firebug-to-ie，等一会在网页的右下角就会出现firebug的图标（如果没有出现图片，点击F12可以直接调用），点击图标，就可以用firebug调试网页了。
2、对于自己制作的网页可以在调试时，先在head部分加入下面的代码，调试成功后再删除。

    <script type="text/javascript" src="https://getfirebug.com/firebug-lite.js"></script>
