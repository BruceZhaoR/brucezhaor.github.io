---
layout:     keynote
title:      "Slide: Data I/O and manipulation in R"
subtitle:   "第一次尝试用revealjs做的html5的slide."
description: "这是我用RStudio开发的revealjs做的slide, 期间还给其贡献了一些代码, 自己还做了一个新的模板. :grin: 想要瞧瞧究竟有多炫酷, 请大胆戳我."
iframe:     "https://brucezhaor.github.io/my-slides/data-manipulation.html#/"
author:     "Bruce Zhao"
tags:        [slides]
categories: [R]
---

> 更多说明往下翻

<ul>
<li><code>S</code> 演讲者模式</li>
<li><code>O</code> 快速浏览模式</li>
<li><kbd>Alt + click</kbd> 放大</li>
</ul> 

这个是我第一次尝试用revealjs做的html5的slide, 期间还给[revealjs](https://github.com/rstudio/revealjs)贡献了一些代码, 然后自己fork开发了新的模板.想要尝试新模板的可以这样下载来玩一玩: <code>devtools::install_github("BruceZhaoR/revealjs")</code>.

安装完毕后, 直接点new rmarkdown -> form template -> revealjs , 然后你就会发现做的一些设置,然后你参考[Rmarkdown](http://rmarkdown.rstudio.com/revealjs_presentation_format.html)官方说明进行设置, 更全的在[github主页上](https://github.com/rstudio/revealjs/blob/master/README.md).

本slide制作说明在[这里](https://github.com/BruceZhaoR/my-slides/blob/master/revealjs/instruction.md), 制作模板在[这里](https://github.com/BruceZhaoR/my-slides/blob/master/revealjs/template.Rmd).

最后, 我想说的是 Rstudio 开发的这个revealjs并没有原生的[reveal.js](https://github.com/hakimel/reveal.js)灵活, 功能也少了一点点, 感觉在里面插入代码看起来有点怪怪的感觉, 感觉只适合快速做个网页版的PPT. 想要更精致, 更多特性的还是建议抄起键盘写html吧.