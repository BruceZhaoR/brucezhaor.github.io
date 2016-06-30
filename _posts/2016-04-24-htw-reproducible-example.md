---
layout: post
title: R——正确提问的姿势
subtitle:   "Hadley教你如何规范格式，让别人快速get到你的问题，从而高效给出解决方案."
categories: [R]
tags: [Misc]
date:       2016-04-24
author:     "Bruce Zhao"
header-img: "/img/post/baracktocat.jpg"
description: "我们在使用R的时候总是会碰到各种报错。怎么折腾都解决不了的时候，只能向大神们求助了。经常有人一上来就一个截图，然后问我这是怎么回事，怎么办之类的。要是简单一点的一眼就能看出来，但是不能一眼就看出来的，我只有将截图上的代码手敲一遍，看看问题出在哪里。这样耗时费力，所以推广正确的提问姿势非常必要的。"
---


## 写在前面的话

我们在使用R的时候总是会碰到各种报错。怎么折腾都解决不了的时候，只能向大神们求助了。经常有人一上来就一个截图，然后问我这是怎么回事，怎么办之类的。要是简单一点的一眼就能看出来，但是不能一眼就看出来的，我只有将截图上的代码手敲一遍，看看问题出在哪里。这样耗时费力，所以推广正确的提问姿势非常必要的。

偶然看到了Hadley写的高级R教程[How to write a reproducible example ](http://adv-r.had.co.nz/Reproducibility.html)里面的关于提问题的规范的文章，感觉很多地方自己也没有做对，故特地来翻译给大家看看，与大家共勉，一起来推广R提问题的正确姿势。

## 如何写一个可复用的例子

当你在使用R的时候，如果你能提供一个可以复用的例子将会得到更好的帮助。一个可复用的例子可以让其他人只需要复制粘贴你的代码就可以重现你的问题。

让你的例子能够复用需要四点：

1. 所需加载的包
2. 数据集
3. 代码
4. 运行环境的描述

- 将所需要加载的包，放在脚本代码的顶部，会让别人清楚地知道问题所依赖的包。
- 使用`dput`函数是最简单的重现数据集的方法。下面举例子说明如何在R中使用代码重构mtcars这个数据集:

1. 在R中运行`dput(mtcars)`
2. 复制输出的结果
3. 在你的可重复例子的脚本中输入`mtcars <-`,然后粘贴过来。

- 花一点时间修改一下你的代码，确保别人能轻松的读懂：
- 确保代码和变量的简洁性
- 用注释来说明的出问题的地方
- 尽你的最大努力来去掉那些于问题不想关的东西，代码越短越容易理解.
- `sessionInfo()`这个函数能够输出你的R环境重要的信息，也方便检查你用的的包是不是过时了。

你可以通过重启R, 快捷键是<kbd>Ctrl+Shift+F10</kbd>, 粘贴你的代码，来确保你成功地写了一个可复用的例子。

在你将你的代码粘贴各大论坛之前，我的建议是将它们放到 <http://gist.github.com/> 上面去。这个会让高亮你的代码，让你的输出更加漂亮，而且不用担心放在其他编辑器中出现格式错乱的情况。

## 例子

这个例子将简单的说明如何创建一个可复用的例子。首先，将你的数据集用`dput()`函数输出，然后复制输出的内容；

{% highlight R %}

# For this example, use the built-in BOD data set. Replace this with your data.
dput(BOD)
#> structure(list(Time = c(1, 2, 3, 4, 5, 7), demand = c(8.3, 10.3, 
#> 19, 16, 15.6, 19.8)), .Names = c("Time", "demand"), row.names = c(NA, 
#> -6L), class = "data.frame", reference = "A1.4, p. 270")

{% endhighlight %}

接着，你可以利用这个输出来创建一个可复用的例子：

{% highlight R %}

library(ggplot2)

# Save the data structure in variable BOD
BOD <- structure(list(Time = c(1, 2, 3, 4, 5, 7), demand = c(8.3, 10.3,
19, 16, 15.6, 19.8)), .Names = c("Time", "demand"), row.names = c(NA,
-6L), class = "data.frame", reference = "A1.4, p. 270")

# Some example code that uses the data
ggplot(BOD, aes(x=Time, y=demand)) + geom_line()

{% endhighlight %}


最后在新的R session里面跑一遍代码，确保其他人也能运行代码。

## 最后

以上就是Hadley关于规范化R提问的建议。国内关于R的资料也是越来越多，统计之都或者百度搜不到的话，建议去[stackoverflow](http://stackoverflow.com/)搜索答案。这个是个全英文的网站，所以你需要将你的问题用几个英文来关键字来概括，最好加上相关的标签，如` [r] [ggplot2] `之类的，增加找到的几率。
