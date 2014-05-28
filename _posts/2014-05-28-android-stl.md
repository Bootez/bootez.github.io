---
layout: post
title: "Android STL"
description: "在Android Native代码中使用STL的问题"
category: 
tags: [android, error]
---
{% include JB/setup %}

今天在做Tesseract OCR到Android的移植时碰到｀fatal error: vector: No such file or directory`的问题。Google一番后发现很多同学也遇到相同的问题，不过大部分人是用NDK编译时碰到的，个人比较喜欢
在源码中编译native代码。在这里关于这两种方法都做个总结：

### 1.NDK编译

官方的说法是只需要在Application.mk文件中添加如下一行即可, 可以参考[这个回答。](http://stackoverflow.com/questions/4893403/cant-include-c-headers-like-vector-in-android-ndk)

{% highlight make %}

#Application.mk
APP_STL := stlport_static

{% endhighlight %}

示例源码下载：

{% highlight sh %}

git clone https://github.com/Bootez/hello-stl-ndk.git

{% endhighlight %}

### 2.源码编译
这个网上给出的回答比较少，因此我直接到Android 4.3 AOSP源码的framework目录中搜寻。一番cgrep后果然发现很多module使用vector，顺藤摸瓜找到它的Makefile，发现它包含了这么一行：

{% highlight make %}

include external/stlport/libstlport.mk

{% endhighlight %}

我们可以直接参照它的方法使用STL了。废话不多说，直接上源码.

示例源码下载：

{% highlight sh %}

git clone https://github.com/Bootez/hello-stl.git

{% endhighlight %}
