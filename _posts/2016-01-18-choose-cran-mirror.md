---
layout: post
title: 如何选择最快的CRAN Mirror下载R包?
subtitle:   "寻找被隐藏的秘密镜像"
categories : [R]
tags : [Misc]
date:       2016-01-18
author:     "Bruce Zhao"
description: "前一阵子帮别人安装Rstudio的时候，发现在选择CRAN Mirror只有清华大学的镜像网站可以选，但是明明自己的USTC的镜像是可以用的，而且非常的快，于是我就来一探究竟。"
---

## 起因

目前在下载packages的时候发现大多数人的CRAN Mirror都只有一个选项——"[China Beijing4](https://mirrors.tuna.tsinghua.edu.cn/)-TUNA Team,Tsinghua University".感兴趣的同学请点这里: <https://mirrors.tuna.tsinghua.edu.cn/>。 第三个就是我们CRAN镜像了，点击就进入了我们熟悉的r-project.org的主页了。

通过简单的了解，发现这个镜像是由清华大学电脑协会维护的，现任会长是Justin Wong。其网站的目的是方便清华学子，对于无法接入清华大学校园网的童鞋们，其速度是明显打了折扣的。

* 对比测试对象：[USTC的镜像](https://mirrors.ustc.edu.cn/CRAN/)
* 时间：几周前
* 地点：上海

当然离北京近的地方网速肯定是要快一些的。USTC镜像在安徽，离上海还是比较近的，所以网速飞起，一个BH包(14M)秒下。

为此我去查了TUNA的官方[微博](http://weibo.com/u/5402274706?is_hot=1#1453126715315),其中一条微博引起了我的注意：

<a target="_blank" href="http://weibo.com/u/5402274706?is_hot=1#1453128394013"><img src="/img/post/cran_mirror/tuna.png"></a>

看了这条微博，我就不想说什么了，毕竟学生党们也是不容易。我去查了一下R在中国的镜像网站，其实一共有7个，其中https的有两个——**TUNA** & **USTC** 。点击图片跳链接：

<a target="_blank" href="https://cran.r-project.org/mirmon_report.html#ca"><img src="/img/post/cran_mirror/china_mirrors.png"></a>

那么问题来，这些隐藏的镜像网站怎么加入选项中呢？下面小赵就来讲讲他是怎么做的。

## 过程

首先，几个函数非常重要：

{% highlight r %}
> getOption("repos")
                                CRAN 
 "https://mirrors.ustc.edu.cn/CRAN/" 
                           CRANextra 
"http://www.stats.ox.ac.uk/pub/RWin" 
attr(,"RStudio")
[1] TRUE

>?chooseCRANmirror 
>?options(repos =)

{% endhighlight %}

通过`getOption("repos")`函数知道目前的镜像网站是哪里的，比如小赵的就是USTC中科大的镜像网。然后通过`?chooseCRANmirror`在帮助文档中可以看到所有的镜像网站在本地的一个csv中，`R_HOME/doc/CRAN_mirrors.csv`。但是R_home又是什么鬼？其实就是你的R的安装路径。

{% highlight r %}

> R.home()
[1] "E:/R/R-32~1.3"

{% endhighlight %}

现在你可以去到那个CRAN_mirrors.csv文件了。你会发现China有7个镜像，就跟我前面的截图一样。那么如何选择一个离自己最近的网站呢？这需要第三个函数`options()`。帮助文档中可以看到：

```
repos:
URLs of the repositories for use by update.packages. 
Defaults to c(CRAN="@CRAN@"), a value that causes some utilities to prompt for a CRAN mirror. 
To avoid this do set the CRAN mirror, by something like 

local({r <- getOption("repos"); 
r["CRAN"] <- "http://my.local.cran"; 
options(repos = r)}).

```

于是小赵就知道用法了，把最后那三个命令修改一下，就能选择离自己最近的镜像网站了。但是经小赵测试，退出后，镜像网站有变成了TUNA了。所以需要小赵在第一篇博客中提到的[方法](http://brucezhaor.github.io/r/2015/12/02/first-blog.html).

## 提高

这个方法就是找到配置文件，写入命令，这样R打开就能配置好你需要的环境，无需重复输入。具体方法为：在你的 R.home/etc 中找到Rprofile文件，仔细阅读英文你会发现，这个就是一个需要自定义的文件。

> Things you might want to change

小赵一眼就看到了那三行，于是就将 **"http://my.local.cran"** 替换成为了 **"https://mirrors.ustc.edu.cn/CRAN/"** ,并且把那三行的注释符号——# 去掉，大功就告成了。

重新启动R，测试一下，如果你碰到了问题就在下面留言，反正小赵是没有问题的。

## 结束

其实R中有很多黑魔法，需要自己去研究探索。有了新的成果最好能找一个载体*(比如小赵的博客)*记录下来，要不然时间一长就忘记了。比如我写这篇博客的时候，已经是过了好几个星期了，几乎又是重头来了一遍，占用了一晚上的时间。所以，坚持写博客将是我2016年的核心计划之一。最后欢迎大家在下面多说评论框发表意见，与我交流~






