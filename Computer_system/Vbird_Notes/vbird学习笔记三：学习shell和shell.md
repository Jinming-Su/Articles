---
title: vbird学习笔记三：学习shell和shell
date: 2017-03-02 00:01:01
categories:
- Computer_system
- Vbird_Notes
tags:
- Linux
description: 《鸟哥的Linux私房菜》学习笔记
---

> 这一部分主要是学习和使用shell，由于之前已经写过一些shell，这一部分就写的比较少，本部分要注重实践哦.

* 认识shell
    * 查看本机安装的shell
    
        ```
        ➜  ~ cat /etc/shells 
        # /etc/shells: valid login shells
        /bin/sh
        /bin/dash
        /bin/bash
        /bin/rbash
        /bin/zsh
        /usr/bin/zsh
        ```
    * 查看`/etc/passwd`可以看到，每个用户登录时都会使用某个默认的shell
* shell教程
    * shell基础<a href="http://blog.csdn.net/u014451076/article/details/50773142"><font color="blue">http://blog.csdn.net/u014451076/article/details/50773142</font></a>
    * shell变量<a href="http://blog.csdn.net/u014451076/article/details/50776232"><font color="blue">http://blog.csdn.net/u014451076/article/details/50776232</font></a>
    * shell运算符<a href="http://blog.csdn.net/u014451076/article/details/50811161"><font color="blue">http://blog.csdn.net/u014451076/article/details/50811161</font></a>
    * 环境变量配置文件<a href="http://blog.csdn.net/u014451076/article/details/50811347"><font color="blue">http://blog.csdn.net/u014451076/article/details/50811347</font></a>
    * 重定向、管道符、通配符<a href="http://blog.csdn.net/u014451076/article/details/50774671"><font color="blue">http://blog.csdn.net/u014451076/article/details/50774671</font></a>
    * 正则表达式<a href="http://blog.csdn.net/u014451076/article/details/50812159"><font color="blue">http://blog.csdn.net/u014451076/article/details/50812159</font></a>
    * 条件判断(1)<a href="http://blog.csdn.net/u014451076/article/details/50812748"><font color="blue">http://blog.csdn.net/u014451076/article/details/50812748</font></a>(2)<a href="http://blog.csdn.net/u014451076/article/details/50815028"><font color="blue">http://blog.csdn.net/u014451076/article/details/50815028</font></a>
    * 循环<a href="http://blog.csdn.net/u014451076/article/details/50815663"><font color="blue">http://blog.csdn.net/u014451076/article/details/50815663</font></a>
* bash shell的内置命令type
    * 使用命令`man bash`，发现输出中包含cd等其他的命令
    * cd等命令实质是bash的内置命令，如果使用`man cd`则提示查找不到，这中情况下可以通过shell的内置命令type查询.
    * type [-tpa] name
    * 参数
        * -t .通过一下显示命令的意义:(1)file，表示外部命令(2)alias: 表示其他命令的别名(3)builtin: 表示内置命令
        * -p: 如果为name外外部命令，会显示完整文件名
        * -a: 从PATH定义的路径中，将所有含有name的命令列出来.
* shell script的跟踪与调试
    * sh [-nvx] x.sh
    * 参数
        * -n: 不执行script，仅检查语法的问题
        * -v: 在执行script前，先将script的内容输出到屏幕上
        * -x: 将使用到的script内容显示到屏幕上
