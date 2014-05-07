---
layout: post
title: "Git server on debian"
description: "本文主要介绍如何利用git与ssh在debian wheezy上搭建git服务器..."
category: Tutorial
tags: [git]
---
{% include JB/setup %}

### Overview
关于Git的使用以及介绍，本文就不缀述了，本文主要讲解如何利用Git和SSH搭建Git服务器进行小规模团队的代码管理。

### Environment：
{% highlight sh %}

uname -a
Linux debian 3.2.0-4-amd64 #1 SMP Debian 3.2.54-2 x86_64 GNU/Linux 

git --version
git version 1.7.10.4

{% endhighlight %}

### Steps：

#### 1.创建Git账户专门用于代码管理的账户
{% highlight sh %}

sudo adduser git

{% endhighlight %}
#### 2.安装SSH
{% highlight sh %}

sudo apt-get install ssh

{% endhighlight %}

#### 3.生成SSH公钥
{% highlight sh %}

ssh-keygen

{% endhighlight %}

#### 4.修改SSH配置文件(~/.ssh/config)

{% highlight sh %}

Host        gitserver
    HostName        192.168.0.1
    Port            22
    User            git
    IdentityFile    ~/.ssh/id_rsa

{% endhighlight %}

{% highlight sh %}
备注：
HostName     -- Git服务器IP
Port         -- 默认端口号
Use          -- Git服务器上的用户名
IdentityFile -- 公钥文件位置

{% endhighlight %}

#### 5.Copy公钥到Git Server上

{% highlight sh %}

ssh-copy-id -i ~/.ssh/id_rsa.pub git@gitserver

{% endhighlight %}

#### 6.登录到Git Server并创建仓库

{% highlight sh %}

ssh git@gitserver
cd
git --bare init repo.git

{% endhighlight %}

#### 7.Clone远程仓库并提交代码

{% highlight sh %}

git clone ssh://git@gitserver/home/git/repo
cd repo
echo "Test" > abc.txt
git add .
git commit -m "Test commit"
git push origin master

{% endhighlight %}
