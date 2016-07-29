---
layout: post
title:      "R批量处理txt文件并写入MySQL"
subtitle:   "填坑以及测试各种常见的方法的性能"
categories: [R]
tags:       [dplyr,DataBase,data.table]
mathjax:    false
date:       2016-07-29
author:     "Bruce Zhao"
bg-color: "linear-gradient(to right, #fff,#808080, #000)"
description: "用R批量读取txt文件，然后将其写入MySQL数据库中。但是事实远远没有这么简单，因为涉及到中文的问题！"
---

io <- microbenchmark::microbenchmark(
  dt = lapply(filelist, function(x){
    fread(x,sep = "\t",header = F,encoding = "UTF-8",colClasses = c(
      rep("character",8),"integer",rep("character",7)))}) %>% rbindlist(),
  readr = lapply(filelist, function(x){
    read_tsv(x, col_names = F,col_types =cols(.default = "c"))}) %>% do.call("bind_rows", .),times = 10L)

io1 <- microbenchmark::microbenchmark(
  RMySQL=dbWriteTable(conn,"rmysql_speed",test_speed,append=T),
  RODBC=sqlSave(odbc,test_speed,"rodbc_speed",append = T),times = 2L
)

microbenchmark::microbenchmark(mysql=dbGetQuery(conn,"select * from test_speed"),
                               odbc = sqlQuery(odbc,"select * from test_speed"),
                               times = 10L)


## 目录
{: .no_toc}

* 目录
{:toc}

## 开始

{% highlight r %}

> io
Unit: milliseconds
  expr       min         lq      mean     median        uq       max neval cld
    dt   76.7573   78.79884  117.4787   83.89957  106.2591  308.0055    10  a 
 readr 2138.4939 2152.19588 2242.2610 2179.00240 2257.3742 2688.8862    10   b
 
 
 > io1
Unit: seconds
   expr        min         lq       mean     median        uq       max neval cld
 RMySQL   2.180623   2.180623   2.352721   2.352721   2.52482   2.52482     2  a 
  RODBC 322.617982 322.617982 406.036914 406.036914 489.45585 489.45585     2   b
 
 
 > microbenchmark::microbenchmark(mysql=dbGetQuery(conn,"select * from rmysql_speed"),
+                                odbc = sqlQuery(odbc,"select * from rmysql_speed"),
+                                times = 10L)
Unit: milliseconds
  expr       min       lq     mean    median        uq       max neval cld
 mysql  66.04235  67.1841 280.9428  69.60182  72.41639 2183.3346    10   a
  odbc 331.52299 340.1690 425.7488 393.86295 525.25158  563.8237    10   a
{% endhighlight %}



