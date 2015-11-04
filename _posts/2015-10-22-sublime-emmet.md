---
layout: post
title: "前端开发神器-emmet"
description: "sublime插件-emmet"
keywords: "插件"
category: 前端
tags: [前端]
---
{% include JB/setup %}

###前端开发神器-emmet

所谓工欲善其事，必先利其器。作为一个前端开发er,IDE没几款惊天地泣鬼神的插件都不好意思说自己是个coder。今天给大家讲解emmet(zenCoding)的使用。有了这款神器，攻城狮们写页面DOM的效率能提高到令人发指的程度，balabala

<!-- more -->

####生成id为container,class为mystyle的div

	//在页面输入
	div#container.mystyle 
	//或者输入
	.mystyle#container
	//然后按下ctrl+e

生成的页面结构下：

	<div class="mystyle" id="container"></div>

####生成class为list，含有5个class为sub的ul标签

	ul.list>li.sub*5

利用子节点 > 生成的页面结构下：

	<ul class="list">
		<li class="sub"></li>
		<li class="sub"></li>
		<li class="sub"></li>
		<li class="sub"></li>
		<li class="sub"></li>
	</ul>

兄弟节点：

	div.container>p.para+a.turn
	//生成
	<div class="container">
			<p class="para"></p>
			<a href="" class="turn"></a>
	</div>

####自动列表记数

如果你需要按顺序生成HTML元素，这个技巧你一定喜欢，使用$符号可以帮助你生成一系列数字，支持class，id，属性，内容等等。如下：
	
	ul>li.item$*3
	//生成如下
	<ul>
		<li class="item1"></li>
		<li class="item2"></li>
		<li class="item3"></li>
	</ul>

###总结

emmet的功能不仅于此，本人列出常用的一些技巧，其他功能参考<a href="http://blog.csdn.net/lmmilove/article/details/9181323" style="color:red;">emmet常用功能</a>。善于利用工具能腾出更多的时间来创造出更大的价值
	

