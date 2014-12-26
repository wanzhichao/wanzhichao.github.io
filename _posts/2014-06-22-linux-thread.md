---
layout: post
title: "php+shell实现多线程demo"
description: "多线程"
keywords: "php多线程"
category: linux
tags: [php,shell,多线程]
---
{% include JB/setup %}

###linux下借助shell实现php多线程

在一个程序中，这些独立运行的程序片段叫作多线程，利用它编程的概念叫做“多线程处理”。具有多线程能力的计算机因有硬件支持而能够在同一时间执行多个线程，进而提升处理性能。php本身不支持多线程，但apache和linux支持多线程。本文主要讲在linux环境下，借助shell脚本实现php多线程。

<!-- more -->

写个简单的demo,源文件test.php

    <?php
    for($i = 0;$i < 10;$i++)
    {
       echo $i;
       sleep(5);//相当于定时器
    }
    ?>

shell脚本文件thread

    #!/bin/bash
    for i in 1 2 3 4 5 6 7 8 9 10
    do
        /usr/bin/php -q /var/www/test.php &
    done


运行shell脚本命令

    sh thread

输出效果

  ![蔡金林的博客之php多线程](/assets/images/thread.png)

可以看到有10个进程同时在进行，定时请求这个shell，在shell的循环中不必每次等php的代码全部执行完再请求下一个文件，而是同时进行。
