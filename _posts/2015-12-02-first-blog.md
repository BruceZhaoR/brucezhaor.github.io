---
layout: post
title: 如何设置R中的TimeZone
subtitle:   "一打开R就自动默认加载"
categories : [R]
tags : [Misc]
date:       2015-12-02
author:     "Bruce"
header-img: "/img/post/firstblog-bg.jpg"
description: 第一篇blog，关于配置R中的启动变量。
---


## 写在前面的话

欢迎大家来到小赵的blog，这是我的第一篇文章。
    
这篇文章将简单记录如何配置**R**中默认启动项。前一阵子使用了`devtools`包里面的一个`session_info()`函数，发现自己的`tz`设置成为了`Asia/Taipei`，用`sessionInfo()`也是可以看到的。在强迫症的作用下，我就要去把`tz`设置成`Asia/Shanghai`.

代码为：
{% highlight r %}

> Sys.timezone()
[1] "Asia/Taipei"
> devtools::session_info()
Session info --------------------------------------------------
 setting  value                         
 version  R version 3.2.2 (2015-08-14)  
 system   x86_64, mingw32               
 ui       RStudio (0.99.484)            
 language (EN)                          
 collate  Chinese (Simplified)_China.936
 tz       Asia/Taipei                   
 date     2015-12-02                    
Packages ------------------------------------------------------
 package  * version date       source        
 devtools   1.9.1   2015-09-11 CRAN (R 3.2.2)
 digest     0.6.8   2014-12-31 CRAN (R 3.1.2)
 memoise    0.2.1   2014-04-22 CRAN (R 3.1.2)
{% endhighlight %}

## 设置R环境

现在开始进入正题，如何设置R中的时区，以及让R每次打开都自动配置好工作环境？

{% highlight r %}
Sys.setenv(TZ="Asia/Shanghai")
{% endhighlight %}

一行命名搞定。（看着简单，我看了好几篇Google的文章）

### startup with custom R environment

上面的命名只是一次性的，当你关掉R再打开，输入`Sys.timezone()`又是`Asia/Taipei`，非常讨厌 ~~所以我的一劳永逸的方法是：

> 在`R.home`里面找到Rprofile文件，在里面加入命名就行了，以后每一次打开首先运行里面的代码，所以就ok了~   

> R.3.2.2\etc\Rprofile.site  加入 `Sys.setenv(TZ="Asia/Shanghai")` 

这个也给我一个启示，比如你经常要加载Hadley的包，你可以直接写在里面，下次一打开R就自动加载好了。

### 编码环境问题
输入：
{% highlight r %}
> sessionInfo()
R version 3.2.2 (2015-08-14)
Platform: x86_64-w64-mingw32/x64 (64-bit)
Running under: Windows 8 x64 (build 9200)

locale:
[1] LC_COLLATE=Chinese (Simplified)_China.936 
[2] LC_CTYPE=Chinese (Simplified)_China.936   
[3] LC_MONETARY=Chinese (Simplified)_China.936
[4] LC_NUMERIC=C                              
[5] LC_TIME=Chinese (Simplified)_China.936    

attached base packages:
[1] stats     graphics  grDevices utils     datasets  methods   base     

loaded via a namespace (and not attached):
[1] tools_3.2.2
{% endhighlight %}

你会看到.936...这个是GBK编码的环境。想必很多人都碰到中文乱码的问题，小赵也是深受其害啊。下一期找一个时间写一写解决中文乱码问题的总结，把我之前探索的经验跟大家交流一下。另外 还有黑魔法用R生成**中文PDF**以及**中文beamer**教程。

## 写在后面的话

这篇非常短的文章希望能给需要的人一点小帮助，同时也作为我的备忘。另外这一版的blog界面布局在手机端上面显示不是很好，这个周末我将会改一改。
下一篇将给大家带来 用**R**连接数据库教程，轻轻松松处理几十万条数据。接下要写的文章包括的内容有：

- 用SQLite搭建本地轻型数据库
- R连接数据库 RSQLite
- dplyr 调用数据库
- readr 方便快捷读入写出数据
- jekeyll 如何搭建像小赵这样的blog（这个主题没有选好，建议大家去这里 https://github.com/jekyll/jekyll/wiki/Themes 挑）
- 用R生成中文PDF
- 用R生成中文beamer
- 可视化专场

干货多多，欢迎大家RSS我的blog。需要拍砖的请在下面`多说`区域评论,可以采用访客模式，但是还是建议去注册一个账号再来评论，支持主流社交账号。
