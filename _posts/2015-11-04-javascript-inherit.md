---
layout: post
title: "javascript实现继承的几种方式"
description: "javascript继承"
keywords: "继承"
category: javascript
tags: [javascript]
---
{% include JB/setup %}

###javascript实现继承的几种方式

jquery的口号是“write less,do more”,时间宝贵、青春无价。很多人会尽可能少的编写代码，并且尽可能多的复用别人写过的优质代码，而继承显然是实现代码高效复用的一种很好的方式。javascript不像java、c++此类编程语言中存在extend关键字，javascript继承都是通过自己写的方法实现，下面为大家介绍博主所知道的几种继承方式

<!-- more -->

####   使用类式继承的预期结果
	
	function Parent(name) {
		this.name = name || 'bob';
	}
	Parent.prototype.say = function() {
		return this.name
	}
	function Child() {}
	//实现继承
	inherit(Child, Parent);

####   1：原型链继承

	function inherit(Child, Parent) {
		Child.prototype = new Parent();
	}

该方法实现比较简单，但缺点也显而易见--不支持将参数传递到子构造函数中，而子构造函数然后又将参数传到父构造函数中，且创建多个Parent对象造成性能下降，考虑以下例子：

	var child = new Chlid('jack');
	child.say(); //输出 ‘bob’

####   2：借用构造函数
	
	function Child(name) {
		Parent.call(this, 'jack');
	}
	new Child().name   //jack
	new Child().say    //undefined

该方法能获得继承成员的副本，但是却无法继承父类的原型

####   3：组合方式
	
	function Child() {
		Parent.call(this, 'jack');
	}
	Child.prototype = new Parent();

使用该方法时应当将复用的成员放到Parent的原型中，这种方式的缺点是，父构造函数被调用两次，造成效率低下。

####   4：临时构造函数

	var inherit = (function(){
		var F = function() {};
		return function(C, P) {
			F.prototype = P.prototype;
			C.prototype = new F();
			C.constructor = C;
		};
		}())

这种方式被称为圣杯模式，是一种比较好的解决方案，而且该模式可以避免在每次继承时都创建临时代理函数(F)

####   5：拷贝复制--深拷贝

	function extentDeep(parent, child) {
		var i,
			toStr = Object.prototype.toString,
			aStr = '[object Array]';

		child = child || {};

		for(i in parent) {
			if(parent.hasOwnProperty(i)) {
				if(typeof parent[i] === 'object') {
					child[i] = (toStr.call(parent[i]) === aStr) ? [] : {};
					extentDeep(parent[i], child[i]);
				}else{
					child[i] = parent[i];
				}
			}
		}
		return child;
	}


		
###总结

继承的方式有多种，实际开发中采用哪种模式，取决于具体的需求。
今日鸡血----keep on fighting!!!
	

