---
layout: post
title:      "Rmd生成中文Beamer"
subtitle:   "R黑魔法系列之生成中文PDF/Beamer"
categories: [R]
tags:       [Rmd]
mathjax:    false
date:       2016-06-30
author:     "Bruce Zhao"
header-image: null
description: "不知道大家有没有看过那种PDF的幻灯片? 小赵觉得学术氛围浓厚,拿这样的能插入R代码和生成的图片的PDF幻灯片去交流,逼格瞬间就能提升一个档次.废话少说,直接上图让你们来感受一下."
---

<style>header.intro-header {background-image: linear-gradient(30deg, #155799, #159978)}</style>

## 目录
{: .no_toc}

* 目录
{:toc}

## 开始

不知道大家有没有看过那种PDF的幻灯片? 小赵觉得学术氛围浓厚,拿这样的能插入R代码和生成的图片的PDF幻灯片去交流,逼格瞬间就能提升一个档次.废话少说,直接上图让你们来感受一下.

## 截图

项目地址在这里: <https://github.com/BruceZhaoR/Zh-beamer>

![img1](/img/post/beamer/beamer1.png)
![img2](/img/post/beamer/beamer2.png)
![img3](/img/post/beamer/beamer3.png)
![img4](/img/post/beamer/beamer4.png)

完整PDF在[这里](https://rawgit.com/BruceZhaoR/Zh-beamer/master/test.pdf)

## 总结

生成中文beamer我是去年研究的,总是感觉LaTeX太死板了,参数太难调整了.比如我要自定义某个幻灯片的字体大小,目前只有只能很复杂的通过设置hook来解决.连谢益辉自己都不建议用beamer,还是HTML5的幻灯片好啊!!! 所以也没有继续研究下去了,感觉没有前途啊...下次有机会我来做一个炫酷的reveal.js的幻灯片.
