---
layout: post
title: "js事件冒泡和事件绑定"
description: "js事件冒泡和事件绑定"
keywords: "冒泡"
category: javascript
tags: [javascript]
---
{% include JB/setup %}

###事件委托&事件绑定

今日在一个项目中遇到如下情况:有一个ul元素，

<!-- more -->

	<ul id="parent-list">
		<li id="post-1">Item 1</li>
		<li id="post-2">Item 2</li>
		<li id="post-3">Item 3</li>
	</ul>

需要对ul下的每个li元素绑定click事件。想起以前菜鸟阶段看到组里资深前端绑定事件都用的$("ul").delegate('li', 'click', function() {});其实，当初是不明白事件委托所带来的优势的，只觉该方法相对于onclick而言显得有点拖沓。后来询问资深，才明白这是事件委托但也不是了解的特别透彻。

js事件委托所用的原理是事件冒泡，将子元素上发生的事件冒泡到父元素上，通过event.target来判断发生在哪个子元素上。这样的好处有：
	
1：效率上的提升。当li多达1000个的时候，假如遍历ul，为每个绑定事件则会造成巨大的消耗。	然而采用事件委托的却仍只需在父元素绑定一次。

   parent.addEventListener('click', function(e) {
		var target = e.target || e.srcElement;
		if(target.nodeName.toLowerCase() == 'li') {
			console.log(target.id);
		}
	}, false)
	
2:对于新添加的元素，也能处理点击事件(ul中添加一个li)。颇有(一处绑定到处，执行的意思)
	
####bind,live,delegate几个方法的区别
	
1：bind，这种方法比较简单、直接。但缺点也如上诉1.2点。

2：$('li').live('click',function(){alert('That tickles!')})。这种方法将事件冒泡到document上，假如页面层级太高。则会增加响应事件的时间
	
3：$("ul").delegate('li', 'click', function() {})。这种方法直接将子元素的事件交给父元素来处理，综合来说，是一种比较好的解决方案。

####总结

	具体用哪种方法的时候，应当考虑好，该方法是不是适合当前场景，有没有更好地替代方案。

	