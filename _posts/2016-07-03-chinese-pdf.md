---
layout: post
title:      "Rmd生成中文PDF"
subtitle:   "R黑魔法系列之生成中文PDF"
categories: [R]
tags:       [Rmd]
mathjax:    false
date:       2016-07-03
author:     "Bruce Zhao"
bg-color: "linear-gradient(200deg, #7b888e, #0085a1)"
description: " :cn: 既然能生成中文的Beamer PDF幻灯片, 那么是不是也可以生成中文的PDF文档呢? 小赵给出答案是: 可以的, 不过你需要折腾一下 :new_moon_with_face: "
---

## 目录
{: .no_toc}

* 目录
{:toc}

## 开始

中文乱码问题一直是使用Windows的我们一大头疼的事情，小赵被其折磨的苦不堪言，所以借着换了新电脑的这个机会把之前研究的生成中文PDF的方法记录下来。这台新电脑与你们环境比较相似，所以应该不会出现那种坑人的情况：在我电脑上面能够跑出来，换成了你们的电脑就跑不出来了... :dog:

我来大致说一下步骤，然后给大家看看截图，最后总结一下。

项目地址在这里：<https://github.com/BruceZhaoR/Zh-beamer#for-chinese-pdf>

1. 下载`code.Rmd`和`default.latex`到同一个文件夹下
2. 打开 `code.Rmd`, <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>K</kbd> (knit PDF).
3. 不出意外，你会遇到pandoc报错`43`。

说明：之所以会报错是因为你的MikTex缺少`default.latex`所依赖的包，接下你需要做的就是打开`default.latex`文件和`MikTex Packages Manager`，然后看看`\usepacakge{...}` 用到了哪些包，通过package manager去下载相应的包，多试几次，你总会成功的，祝你好运！:wink:

## 截图

目录：
![img1](/img/post/pdf/1.png)
代码：
![img2](/img/post/pdf/2.png)
公式：
![img3](/img/post/pdf/3.png)

## 总结

中文PDF这个也只有在学校的同学用的到了。示例PDF是某位同学的某次作业，题目请见上文提到的文件夹。交上去这么一份颜值巨高的PDF，肯定会给老师留个好印象，自然也会给你高分，哪怕你题目做错了。:blush: 

这里需要更正一下我之前的做法，因为是老电脑，可能存在一些我不知道的环境，然后按我之前的那种方法折腾，在我的电脑里面能跑出来，而在那些向我请教的小伙伴们的电脑里面却跑不出来,所以还没有跑出的小伙伴们快试试新的方法。:joy:

在debug中可能会花掉你很多的时间，但是研究出来了就是一劳永逸的事情，下次生成PDF直接knit一下就ok了。所以，这个还是值得的~
