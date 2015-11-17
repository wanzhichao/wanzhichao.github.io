---
layout: post
title: "编写自己的jquery插件"
description: "jquery插件的编写"
keywords: "插件"
category: javascript
tags: [javascript]
---
{% include JB/setup %}

###编写自己的jquery插件

做项目的过程中总会有些需求是需要私人定制的，比如弹框、图片轮播等，需要按照设计好的效果展现。这时候开发一款属于自己的插件就很有必要，插件开发分为类级别的插件开发和对象级别的插件开发。

<!-- more -->

####一:类级别的插件开发

1：使用jQuery.extend(object)方法，为jQuery添加一些静态方法
	
	jQuery.extend({
		add: function(a, b) {
				return a + b;
			},
		sub: function(a, b) {
				return b - a;
		}
		});
	//调用
	jQuery.add(1, 2); || $.add(1, 2);

2: 使用命名空间的方法，可以大大减少jQuery中的变量名。

	jQuery.myPlugin = {
		add: function(a, b) {
			return a + b;
		}
	}
	//调用
	jQuery.myPlugin.add(1, 2); || $.myPlugin.add(1, 2);

####二:对象级别的插件开发

1:对象级别的插件是将方法绑定到jQuery的原型链上，所以生成的jQ对象也能继承该方法。

	$.fn.extend({
		add: function(a, b) {
				return a + b;
		}
		})
	//调用
	$("#id").add(1, 2);

对象级别的插件开发也可以使用命名空间的形式，<font color="red">但是这样是否就可以了呢</font>

####使用闭包来保护私有函数的私有性

	(function($){
		$.prompt = function(o) {
			//...
		}
		$.extend($.prompt, {
			init: function() {
				//...
			},
			doSomething: function() {
				//...
			}
			})
		})(jQuery);

###总结

动手之前心里畏惧感还是有的，总感觉做一个插件是件很难的事儿，但将事情拆解开来一步步去实现，一切都是水到渠成。设计功能-->查资料-->搭架子-->具体实现-->回顾不足-->继续改进----》》success。

做插件的过程中让我体会到:_合抱之木，生于毫末；九层之台，起于垒土；千里之行，始于足下_。只要一步步循序渐进，脚踏实地，自然<font color="red">_功到而垂成矣_</font>


