---
layout: post
title: "Jekyll Installation"
description: "Jekyll博客系统在Debian Wheezy上的搭建流程记录"
category: software
tags: [linux, jekyll, tutorial]
---
{% include JB/setup %}

### Overview

Jekyll是什么？将纯文本转化为静态博客网站的引擎,发现了类似[官方中文网站](http://jekyllcn.com/)，有兴趣的话可以进一步了解Jekyll。本文主要记录一下在Debian Wheezy上安装Jekyll环境的过程。

### Environment：
{% highlight sh %}

uname -a
Linux debian 3.2.0-4-amd64 #1 SMP Debian 3.2.54-2 x86_64 GNU/Linux 

{% endhighlight %}

### Steps:

#### 1.安装Ruby环境

{% highlight sh %}

sudo apt-get install ruby ruby1.9.1-dev

{% endhighlight %}

#### 2.安装Jekyll
{% highlight sh %}

sudo gem install jekyll rake therubyracer execjs

{% endhighlight %}

#### 3.安装代码高亮插件

{% highlight sh %}

sudo apt-get install python-pygments

{% endhighlight %}
