---
layout: post
title: "Use repo to manage project"
description: "本文主要介绍如何利用repo管理小团队内部的Android代码"
category: software
tags: [linux, repo, tutorial]
---
{% include JB/setup %}

### Overview
关于repo本文就不过多介绍了，repo是一个Python脚本，是Google对git的封装，用来管理AOSP代码的。AOSP代码实际上是由若干个以git为版本控制工具的项目构成。

### Environment
{% highlight sh %}

uname -a
Linux debian 3.2.0-4-amd64 #1 SMP Debian 3.2.54-2 x86_64 GNU/Linux 

git --version
git version 1.7.10.4

{% endhighlight %}

利用repo管理代码有两种情境：
1. 从AOSP git服务器同步代码到内部git服务器上，然后修改manifest.xml中的URL指向内部git服务器；
2.将团队的代码上传到内部git服务器，自己编写manifest.xml文件管理代码。

下面将分两种情况讨论步骤：

**情景一:**

### Steps:

#### 1. 配置git与ssh（可参考我前面的文章[Git Server on Debian Wheezy](http://bootez.github.io/software/2014/05/05/git-server-on-debian-wheezy/)）

#### 2. 创建一个mirror：
{% highlight sh %}

$ mkdir -p /usr/local/aosp/mirror
$ cd /usr/local/aosp/mirror
$ repo init -u https://android.googlesource.com/mirror/manifest --mirror
$ repo sync

{% endhighlight %}

同步完成后你就可以在本地同步这个镜像仓库到本地了：
{% highlight sh %}

$ mkdir -p /usr/local/aosp/master
$ cd /usr/local/aosp/master
$ repo init -u /usr/local/aosp/mirror/platform/manifest.git
$ repo sync

{% endhighlight %}

*Note*
上面的步骤是Google官方copy过来的，尝试后发现这样并不正确，正确的步骤应该是在mirror同步完成后修改manifest.xml指向git服务器IP对。正确的步骤应该是：

{% highlight sh %}

$ mkdir -p /usr/local/aosp/mirror
$ cd /usr/local/aosp/mirror
$ repo init -u https://android.googlesource.com/mirror/manifest --mirror
$ repo sync

{% endhighlight %}

同步完成后你就可以在本地同步这个镜像仓库到本地了：
{% highlight sh %}

$ git clone /usr/local/aosp/mirror/manifests.git
$ cd manifests (修改fetch的URL)
$ mkdir -p /usr/local/aosp/master
$ cd /usr/local/aosp/master
$ repo init -u /usr/local/aosp/mirror/platform/manifest.git
$ repo sync

{% endhighlight %}

**情景二:**
如果代码并不是从AOSP中同步的，而是团队内部的代码，则需要自己手动编写manifest.xml，格式可参考AOSP中的manifest.xml(AOSP/.repo/manifest.xml)
剩下的步骤同情景一。
