---
layout: post
title:      "Rmd生成中文Beamer"
subtitle:   "R黑魔法系列之生成中文PDF/Beamer"
categories: [R]
tags:       [Rmd]
mathjax:    false
date:       2016-06-30
author:     "Bruce Zhao"
bg-color: "linear-gradient(56deg, #155799, #159978)"
description: " :cn: 不知道大家有没有看过那种PDF的幻灯片? 小赵觉得学术氛围浓厚,拿这样的能插入R代码和生成的图片的PDF幻灯片去交流,逼格瞬间就能提升一个档次.废话少说,直接上图让你们来感受一下. :smirk: "
---


## 目录
{: .no_toc}

* 目录
{:toc}

## 开始

不知道大家有没有看过那种PDF的幻灯片? 小赵觉得学术氛围浓厚,拿这样的能插入R代码和生成的图片的PDF幻灯片去交流, 逼格瞬间就能提升一个档次. 废话少说,直接上图让你们来感受一下. :blush:

## 截图

项目地址在这里: <https://github.com/BruceZhaoR/Zh-beamer>

![img1](/img/post/beamer/beamer1.png)
![img2](/img/post/beamer/beamer2.png)
![img3](/img/post/beamer/beamer3.png)
![img4](/img/post/beamer/beamer4.png)

完整PDF在[这里](https://rawgit.com/BruceZhaoR/Zh-beamer/master/test.pdf).

## 总结

最重要的两个文件是 `beamer_default.tex` 和 `_output.yaml`, 新建的Rmd需要跟他们放在一个文件夹下. 你可以自定义主题和字体, 修改的地方可以是`output options` 或者 `_output.yaml`. 更详细的说明情况rmarkdown的官方文档关于beamer的: <http://rmarkdown.rstudio.com/beamer_presentation_format.html>

生成中文beamer我是去年研究的, 总是感觉LaTeX太死板了, 参数太难调整了. 比如我要自定义某个幻灯片的字体大小, 目前只有只能很复杂的通过设置hook来解决. 连谢益辉自己都不建议用beamer, 还是HTML5的幻灯片好啊 !!! 所以也没有继续研究下去了, 感觉没有前途啊 ... 下次有机会我来做一个炫酷的reveal.js的幻灯片.

最后, 如果碰到了别人说: "你这个是PPT转的PDF吧. 小伙子, PPT主题和模板选的不错啊, 给我也来一份吧". 

我想此刻你的心情是这样的: :sob: :new_moon_with_face: .更多[表情包](http://www.emoji-cheat-sheet.com/).
