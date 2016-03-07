---
title: javascript屏蔽Ctrl+s，F1，F3各浏览器兼容写法
tags: 键盘事件、兼容性
grammar_cjkRuby: true
---



近来项目需要用到ctrl+S，F1，F3等快捷键，无奈各浏览器对这几个键都有默认事件，且屏蔽方式不尽相同，现对其作一整理总结。注意：该文是针对原生javascript写法，jquery已对相当一部分做了兼容处理，并不需要如此复杂。

**键盘事件**
        一般处理键盘按键事件我们采用这样的方式
<pre>
  <code class='javascript'>
 document.onkeydown=function (event) { 
     //检测按下哪个键，作相应处理
 };
 </code>
</pre>

其中event为键盘事件，对于chrome，firefox，IE（Edge），IE10，IE9均能支持function自带的e，而ie8以下只能识别windows.event，所以一般兼容写法为：event=event||window.event。获取按键码一般是event.keyCode,这个对各大浏览器都是兼容的。
<pre>
  <code class='javascript'>
document.onkeydown=function (event) { 
     event=event||window.event;
     var key=event.keyCode;
     //检测按下哪个键，作相应处理
     if(key==...){
   } 
 }; 
  </code>
</pre>
屏蔽浏览器默认事件的方法大致有三种：
1）event.preventDefault()
2）event.returnValue=false;
3）return false; 
**下面将逐一分析Ctrl+S，F1，F3键各浏览器对这三个方法的兼容性**
      ctrl+S
          这对组合键我们经常用来做保存，各浏览器的默认事件也是保存当前网页。

|                          | Chrome | Firefox      | IE（Edge） | IE10， IE9   | IE8以下      |
| ------------------------ | ------ | ------------ | ---------- | ------------ | ------------ |
| event.preventDefault()   | 支持   | 特殊方式支持 | 不支持     | 不支持       | 不支持       |
| event.returnValue=false; | 支持   | 不支持       | 不支持     | 特殊方式支持 | 特殊方式支持 |
| return false;            | 支持   | 特殊方式支持 | 支持       | 支持         | 支持         |

 
firefox的特殊方式支持是指，firefox这里一个比较坑爹的地方是，firefox需增加一个延迟才能生效，不然仍然会跳出浏览器的保存当前页面窗口，如下：
<pre>
  <code class='javascript'>
document.onkeydown=function (event) {
            //判断按键  
             var key=event.keyCode; 
            if(key== 83 && e.ctrlKey){
                 /*延迟，兼容FF浏览器  */
                 setTimeout(function(){
                    alert('ctrl+s'); 
                  },1); 
                   event.preventDefault();//或者是 return false;    
              } 
    </code>
</pre>            

而IE10，IE9，IE8以下对于`event.returnValue=false`的特殊方式支持是指键盘事件event必须为window.event时ctrl+s的默认事件才能屏蔽，在`event=event||window.event`的兼容写法中，IE8及以下的形参event是空，所以会取值为window.event,而IE10，IE9的function形参event是有效的，所以取值直接为event，因此IE10，IE9在写法为`event=event||window.event`时会屏蔽ctrl+s失效。

假如要让所有IE版本能够屏蔽Ctrl+S，event取值只能是window.event了。由于window.event没有方法preventDefautl，所以屏蔽默认事件方法只能用return false;
     兼容IE、firefox、chrome，屏蔽Ctrl+s的写法为
<pre>
  <code class='javascript'>
      document.onkeydown=function (e) { 
           e=window.event||e;
             if(key== 83 && e.ctrlKey){
                 /*延迟，兼容FF浏览器  */
                  setTimeout(function(){
                  alert('ctrl+s'); 
                 },1); 
                    return false;      
              }    
           };
   </code>
</pre>      

   F1
   F1的浏览器默认事件是打开帮助页，它的屏蔽方式相对简单，以下三种方式均可屏蔽firefox、chrome的F1事件
1）event.preventDefault()
2）event.returnValue=false;
3）return false; 
特殊的是IE，只需添加一句： 

    window.onhelp = function () { return false };

即可屏蔽IE的F1事件

F3
  F3的浏览器默认事件是查找，它的各屏蔽方法兼容性为：
 

|                          | Chrome | Firefox | IE（Edge） | IE10， IE9 | IE8以下 |
| ------------------------ | ------ | ------- | ---------- | ---------- | ------- |
| event.preventDefault()   | 支持   | 支持    | 支持       | 不支持     | 支持    |
| event.returnValue=false; | 支持   | 不支持  | 支持       | 支持       | 支持    |
| return false;            | 支持   | 支持    | 支持       | 支持       | 支持    |

 
其实，使用return false基本能兼容所有浏览器的默认键盘事件，不过听说return false还会屏蔽冒泡事件，这个暂时还没验证。