---
layout: post
title: "Fcitx on Debian Wheezy"
description: "在Debian Wheezy上安装Fcitx记录"
category: 
tags: [linux, tutorial]
---
{% include JB/setup %}

### Overview

Linux下五笔输入法也尝试过一些，IBus经常出一些莫名其妙的问题，不得不转投Fcitx，不过Fcitx也确实很好用。

### Environment

{% highlight sh %}

uname -a
Linux debian 3.2.0-4-amd64 #1 SMP Debian 3.2.57-3+deb7u2 x86_64 GNU/Linux

{% endhighlight %}

### Steps

#### 1.安装fcitx及五笔的码表

{% highlight sh %}

sudo apt-get install fcitx fcitx-table-wbpy im-switch

{% endhighlight %}

#### 2.设置fcitx为系统默认输入法
{% highlight sh %}

im-switch

{% endhighlight %}

选择fcitx

#### 3.根据自己的需求简单配置，默认配置已经很好用了，外观配置文件的位置在:`~/.config/fcitx/conf`
