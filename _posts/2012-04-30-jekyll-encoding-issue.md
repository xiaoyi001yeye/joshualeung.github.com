---
layout: post
title: 解决Windows中Jekyll由于编码不兼容而无法启动的问题
description: ""
category: Software Tricks
tags: [Jekyll]
---
{% include JB/setup %}

在参照[**Jekyll Quick Start**](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)一文对Jekyll进行安装和配置后，启动Jekyll --server时提示如下错误：
>Forbidden
>
>no access permission to '/'
>WEBrick/1.3.1 (Ruby/1.9.3/2012-04-20) at localhost:4000

在Google上搜索了一下，网上提供的解决方案（参见[这里](https://github.com/imathis/octopress/issues/232)）是修改Jekyll读取文件时的默认编码，即将convertible.rb(去Jekyll安装目录里找吧)中的第29行（左右）由

{% highlight ruby %}
self.content = File.read(File.join(base, name))
{% endhighlight %}

修改为

{% highlight ruby %}
self.content = File.read(File.join(base, name), :encoding => "utf-8")
{% endhighlight %}	

然而，照此修改后，在本地用http://localhost:4000访问时，又出现了新的错误，即：

>liquid error: incompatible character encodings: UTF-8 and GBK

又在Google上搜索了一下，有网友指出通过修改系统环境变量的方式来解决这一问题，即在命令提示符下输入（具体讨论参见[这里](http://ruby-taiwan.org/topics/46)）：

{% highlight sh %}
set LC_ALL=en_US.UTF-8 
set LANG=en_US.UTF-8 
{% endhighlight %}

Bingo!错误终于消灭了，Jekyll的本地Server正常启动。