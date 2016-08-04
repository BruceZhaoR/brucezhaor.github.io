---
layout: post
title:      "R批量处理txt文件并写入MySQL"
subtitle:   "填坑以及测试各种常见的方法的性能"
categories: [R]
tags:       [dplyr,DataBase,data.table]
mathjax:    false
date:       2016-08-04
author:     "Bruce Zhao"
bg-color: "linear-gradient(to right, #fff, #808080, #000)"
description: "用R批量读取txt文件，然后将其写入MySQL数据库中。但是事实远远没有这么简单，因为涉及到中文的问题！"
---


## 目录
{: .no_toc}

* 目录
{:toc}

## 开始

这篇博客主要记录的是批量读入txt文件并整合的常见方法及其速度比较，然后是解决中文不能写入到MySQL数据库小技巧。这里我首先比较的是`readr&dplyr`和`data.table`的读取和拼接速度；其次是比较RODBC和RMySQL这两种不同连接方式将数据写入MySQL的速度，顺便比较读取速度；最后再来简单总结一下。


## 批量读取txt及按行拼接

这里简单粗暴的方法就是写循环，但是不建议这样做，这里小赵给出了使用lapply的方法。读取的函数也不建议使用基础包里面的，小赵推荐有`readr::read_tsv`和`data.table::fread`。`readr`包据说比基础包快10倍及以上，而且较为灵活，可以设置各列的数据类型，更多的说明和用法请去github看看[README](https://github.com/hadley/readr); `fread`据说要比`read_csv/tsv`快2.5倍及以上，但是相对来说没有readr那么多的参数设置，适合快速读入规整的data frame。按行拼接dplyr的是`bind_rows`，data.table的是`rbindlist`。

**数据说明：**用于测试的是129个较为规整的txt文件，一共是16列，导入内存中总共约是14W行的样子，2M多。由于各文件中列的数据类型不太一样，为了方便后续的拼接所以全部设成字符串类型。

**机器环境说明：** i5-6200U, 8G RAM DDR4。

```r
library(readr)
library(dplyr)
library(data.table)

> filelist <- list.files("test-data/", pattern = "\\.txt")  #列出文件夹下所有txt类型的文件

# 用microbenchmark包来进行速度测试
> io <- microbenchmark::microbenchmark(

  dt = lapply(filelist, function(x){
    fread(x, sep = "\t", header = F, encoding = "UTF-8",colClasses = rep("character",16))}) %>% rbindlist(),

  readr = lapply(filelist, function(x){
    read_tsv(x, col_names = F, col_types = cols(.default = "c"))}) %>% do.call("bind_rows", .),

  times = 10L)  # 测试10次

> io
Unit: milliseconds
  expr       min         lq      mean     median        uq       max neval cld
    dt   76.7573   78.79884  117.4787   83.89957  106.2591  308.0055    10  a 
 readr 2138.4939 2152.19588 2242.2610 2179.00240 2257.3742 2688.8862    10   b
  
```

从测试的结果的中位数来看，data.table的速度约比readr快25倍的样子，但是`fread`的波动相对来说比`read_tsv`大很多。我发现一个有意思的现象，fread第一次读取稍微慢一点，到后面会越读越快。

## 写入MySQL数据库

读入内存中的数据集都是`UTF-8`编码的，所以我将MySQL数据库的编码也设置成`utf-8`的编码，然后我就入坑了。之后，我不管是通过odbc还是DBI的方式写入数据库时都碰到了中文无法写入问题。最后，我通过反复测试发现原来是数据库编码的问题，这里需要将数据库的编码设置成`gb18030/gbk`的方式才行。打通接口后，下面来进行写入测试。

R连接数据库有两种常用方法：一种是通过ODBC的方式，通过RODBC包来实现，但是需要自己下载MySQL的odbc驱动然后通过控制面板里面的管理数据源来进行配置；另外一种方式是DBI，通过RMySQL包来实现连通。

```r
library(RODBC)
library(RMySQL)

# 连接方式

conn <- dbConnect(RMySQL::MySQL(), username="root", password="****",
                  host="localhost", port=3306)
# host可以是云端的地址也可以是本地局域网IP地址的，例如"192.168.1.200"这样的。

odbc <- odbcConnect("配置的名称", uid = "", pwd = "", ...)
# 可以直接在管理数据源里面配置好用户名和密码，然后直接连接其名称就行。

# 写入MySQL速度测试

> io1 <- microbenchmark::microbenchmark(
  RMySQL=dbWriteTable(conn,"rmysql_speed",test_speed,append=T),
  RODBC=sqlSave(odbc,test_speed,"rodbc_speed",append = T),
  times = 2L
)
# 需要注意的是两种包写入数据库不同的写法；
# 由于RODBC的方式巨慢，所以只测两次，当然你也可以加大测试次数。

> io1
Unit: seconds
   expr        min         lq       mean     median        uq       max neval cld
 RMySQL   2.180623   2.180623   2.352721   2.352721   2.52482   2.52482     2  a 
  RODBC 322.617982 322.617982 406.036914 406.036914 489.45585 489.45585     2   b
```

根据上面的结果，发现RMySQL的写入速度完爆RODBC的写入速度，所以还是建议使用RMySQL这种连接方式。不过这个可能会暴露你的用户名及密码，当然你可以预先将用户名以及密码设置成变量保存为script然后通过source来调用就可以了。



## 读取速度测试

测试完写入速度，最后顺便来测试一下读取速度。

```r
 
 > microbenchmark::microbenchmark(mysql=dbGetQuery(conn,"select * from rmysql_speed"),
+                                odbc = sqlQuery(odbc,"select * from rmysql_speed"),
+                                times = 10L)
Unit: milliseconds
  expr       min       lq     mean    median        uq       max neval cld
 mysql  66.04235  67.1841 280.9428  69.60182  72.41639 2183.3346    10   a
  odbc 331.52299 340.1690 425.7488 393.86295 525.25158  563.8237    10   a

```

测试10次，从均值来看RMySQL比RODBC 快2倍左右，中位数来看，RMySQL比RODBC快5倍以上，但是其最大值比RODBC大4倍的样子，只能说明RMySQL不是很稳定的样子，可能第一次读取慢点，但是后面就会变的很快。


## 最后

总体来看，RMySQL性能要比odbc的接口要好一些，而且不用下载驱动之类的东西，用起来十分的方便，我个人还是推荐这个。

最后忘了填补一个小坑：数据库中的编码为GB18030所以在读入到R中的时候回乱码，你需要这样的一行命令 —— `dbSendQuery(conn,"set character_set_results=gb18030;")`，然后你就不会出现中文乱码的问题了。Good Luck！ :blush:












