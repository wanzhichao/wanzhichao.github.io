---
layout: post
title: "linux下使用g++编译，gdb调试c++程序"
description: "g++编译，gdb调试c++程序"
keywords: "linux gdb调试c++程序"
category: linux
tags: [g++,gdb]
---
{% include JB/setup %}

### linux下编译调试c++程序

都说不会调试的程序员不是好程序员，今天终于通过看相关博客，进社区，动手实践三大步骤，把linux下编译和调试c++程序过了一遍。
本文主要写linux下使用Vim编辑器写c++程序，然后通过g++编译器对程序进行编译，最后用gdb调试工具进行调速，命令行式的调试，有助于学习并熟知linux环境下c++/c编程操作。

<!-- more -->
一般来说，GCC主要帮忙你完成一下四方面的功能：

    1.预处理
    2.编译
    3.汇编
    4.连接

特点：支持多语言，多系统

一般来说，GDB主要帮忙你完成下面四个方面的功能：

    1、启动你的程序，可以按照你的自定义的要求随心所欲的运行程序。
    2、可让被调试的程序在你所指定的调置的断点处停住。（断点可以是条件表达式）
    3、当程序被停住时，可以检查此时你的程序中所发生的事。
    4、动态的改变你程序的执行环境。

特点:命令行式的调试工具，非图形化操作

编译c/c++程序命令:

	gcc -o demo demo.c
	g++ -g -o demo demo.cpp

gdb基本命令列表：

![蔡金林的博客之GDB命令](/assets/images/gdb.png)

以我在leetcode(https://oj.leetcode.com/)上面AC的一道题目"Reverse Words in a String"来分析：

1.使用vim编辑器编写c++程序

![蔡金林的博客之反转字符串并去掉多余空格类](/assets/images/Reverseclass.png)

主函数main:

	#include<iostream>
	#include<algorithm>
	#include<string>
	int main()
	{
	     string inputstr;
	     getline(cin,inputstr);
	     Solution *s =  new Solution();
	     s->reverseWords(inputstr);
	     delete s;
	     cout<<inputstr<<endl;
	}

2.gcc编译,生成可执行文件,注意必须使用-g参数，编译会加入调试信息否则无法执行文件

	g++ -g -o demo demo.cpp

3.gdb调试

![蔡金林的博客](/assets/images/gdbstart.png)

3.1查看源文件 list 1,回车，直到显示完整程序

![蔡金林的博客](/assets/images/gdblist.png)

3.2设置断点 break 8,在第8行设置断点，info break查看断点信息

![蔡金林的博客](/assets/images/gdbbreak.png)

3.3调试运行 输入run 或者r 

进行程序的输入

	   the sky is   blue 

![蔡金林的博客](/assets/images/gdbrun.png)

3.4单步调试，输入step 或者 s进入函数内部

![蔡金林的博客](/assets/images/gdbstep.png)

3.5查看变量 输入print 变量 或者 p 变量

![蔡金林的博客](/assets/images/gdbprint.png)

3.6继续运行直到下一个断点或主函数结束 输入continue或者 c

程序结果

	blue is sky the

![蔡金林的博客](/assets/images/gdbcontinue.png)


3.7 退出调试 输入q
