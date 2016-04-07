---
layout: post
title: "Git进阶1"
subtitle:   "常用基本操作"
categories : [Git]
tags : [Git]
date:       2016-02-25
author:     "Bruce Zhao"
header-img: "/img/post/git-junior-bg.jpg"
description: "这几个月在使用Git的过程中学到了不少实用的小技能，小赵准备简单总结一下，来与大家分享。也欢迎大家在评论区不断补充~"
---

## 目录
{: .no_toc}

* 目录
{:toc}

## 开始

看了很多的git教程博客，只有极少数是以问题为导向的来介绍git操作命令，大多数只是简单的罗列。然而对于新手来说，不交代具体的使用情景很容易让人理解不了。这也是小赵准备写这样的一篇教程的原因之一，另外还有一个原因是为了备忘与总结。废话少说，现在进入正题。

---
这篇文章也是拖了好久，差不多一个月了。一般开始写的时候都是晚上10点左右，想想觉得要写很久，明天还要上班，刷刷手机就差不多拖到了11点多，这样循环了一个多月。清明节回了趟武汉，也去了学校，与老师交流以及自己思考后有了许多新的感悟，在思想上发生了一些转变。关于这些，我后续整理另写一篇文章发布。

---

## 分支的创建

### gh-pages

通过创建分支，并且命名为`gh-pages`就能生成网站，用于项目展示，或者当做另外一个博客来用。在settings里面可以快速设置项目主页，通过编辑markdown，github直接给你生成一个index.html网页，然后会出现访问的地址。

### 开发新的特性

你突然有了一个新的想法，但是不知道这个新的想法是否可行，你可以创建分支，将目前master里面的内容copy到分支中，然后在此基础上进行开发，而不会影响到主分支master。等开发测试通过后，merge到master中，就能实现新功能的添加。如果不可行，也可以删除分支，总之是不会影响到master的。

### 实例与常用命令

假设你已经clone了master分支到了本地，现在要创建新分支然后查看有关信息。

{% highlight bash %}

git checkout -b gh-pages #新建分支,名为gh-pages,并switch.
git branch -vv #查看分支信息
git remote -v  #查看远程仓库信息
git push origin gh-pages # 将本地新建的分支同步到github上面

{% endhighlight %}


## 添加上游仓库

上游仓库的英文名是`upstreaming repository`,所以在看英文的资料的时候不要装作不认识了。

### 为什么

明天接着写






