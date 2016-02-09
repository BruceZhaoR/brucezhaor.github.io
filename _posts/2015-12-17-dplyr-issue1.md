---
layout: post
title: dplyr连接SQLite数据库
subtitle: "使用dplyr包和RSQLite包实现数据库的连接与管理，轻松处理百M级别数据；数据乱码源头解决方案——readr。"
category : R
tags : [dplyr,database,RSQLite]
layout: post
date:       2015-12-17
author:     "Bruce Zhao"
header-img: "/img/post/dplyr1-bg.jpg"
description:  使用dplyr包和RSQLite包实现数据库的连接与管理，轻松处理百M级别数据；数据乱码源头解决方案——readr。
---


## 写在前面的话
这篇文章为我提交的 [dplyr issue][issue1]，关于连接数据库以及解决中文乱码问题。所以这篇文章主要有三个点：

* dplyr 调用数据库
* R连接数据库 RSQLite
* readr 方便快捷读入写出数据

这些都是基本的处理数据的技能，dplyr包能够处理GB级别的数据，再加上SQLite本地轻型数据库，可以实现数据存储、调用、清洗、处理等一个系列问题。时间有限，背景说明就少说一点，直接上干货。
请对照参考我提交的两个issues:<https://github.com/hadley/dplyr/issues/1561> , <https://github.com/rstats-db/RSQLite/issues/108>.


## 1. 可重复使用的例子

### SQL代码生成需要用的tables

在你的sqlite里面运行下面代码，就能得到例子中用到的两个表。关于搭建sqlite下次有时间再写详细教程，其实非常简单，根本不需要部署，安装了就可以用，可自行百度去。

{% highlight sql %}

-- ----------------------------
-- Table structure for flights
-- ----------------------------

DROP TABLE IF EXISTS "main"."flights";
CREATE TABLE flights(
  year INT,
  month INT,
  day INT,
  dep_time INT,
  dep_delay REAL,
  arr_time INT,
  arr_delay REAL,
  carrier TEXT,
  tailnum TEXT
);

-- ----------------------------
-- Records of flights
-- ----------------------------

INSERT INTO "main"."flights" VALUES (2013, 1, 1, 517, 2.0, 830, 11.0, 'UA', 'N14228');
INSERT INTO "main"."flights" VALUES (2013, 1, 1, 533, 4.0, 850, 20.0, 'UA', 'N24211');
INSERT INTO "main"."flights" VALUES (2013, 1, 1, 542, 2.0, 923, 33.0, 'AA', 'N619AA');
INSERT INTO "main"."flights" VALUES (2013, 1, 1, 544, -1.0, 1004, -18.0, 'B6', 'N804JB');
INSERT INTO "main"."flights" VALUES (2013, 1, 1, 554, -6.0, 812, -25.0, 'DL', 'N668DN');
INSERT INTO "main"."flights" VALUES (2013, 1, 1, 554, -4.0, 740, 12.0, 'UA', 'N39463');

-- ----------------------------
-- Table structure for cor
-- ----------------------------

DROP TABLE IF EXISTS "main"."cor";
CREATE TABLE cor(
  XXDM TEXT,
  XBDM TEXT,
  NJDM TEXT,
  "身体形态" INT,
  "50米跑等级" INT,
  "肺活量等级" INT
);

-- ----------------------------
-- Records of cor
-- ----------------------------

INSERT INTO "main"."cor" VALUES ('0015', 2, 11, 3, 3, 4);
INSERT INTO "main"."cor" VALUES ('0015', 1, 11, 2, 2, 4);
INSERT INTO "main"."cor" VALUES ('0015', 1, 11, 2, 2, 4);
INSERT INTO "main"."cor" VALUES ('0015', 1, 11, 2, 2, 2);
INSERT INTO "main"."cor" VALUES ('0015', 1, 11, 2, 2, 3);
INSERT INTO "main"."cor" VALUES ('0007', 1, 13, 2, 3, 4);
{% endhighlight %}

关于表的一些说明：flights 这个表是 {nycflights13}  package 里面的，描述的是 On-time data for all flights that departed NYC (i.e. JFK, LGA or EWR) in 2013.
cor 这个表是我的sqlite数据库中的表。为了篇幅，只取了前6行。

## 2. dplyr连接数据库

{% highlight r %}
>library(dplyr)
>mydb <- src_sqlite("E:/SQLite/database/yourdbname.db",create = F)
>mydb
src:  sqlite  [E:/SQLite/database/yourdbname.db]
tbls: cor, flights

>tbl(mydb,"flights")
Source: sqlite  [E:/SQLite/database/yourdbname.db]
From: flights [336,776 x 16]
    year month   day dep_time dep_delay arr_time arr_delay carrier tailnum
   (int) (int) (int)    (int)     (dbl)    (int)     (dbl)   (chr)   (chr)
1   2013     1     1      517         2      830        11      UA  N14228
2   2013     1     1      533         4      850        20      UA  N24211
3   2013     1     1      542         2      923        33      AA  N619AA
4   2013     1     1      544        -1     1004       -18      B6  N804JB
5   2013     1     1      554        -6      812       -25      DL  N668DN
6   2013     1     1      554        -4      740        12      UA  N39463

>tbl(mydb,"cor")
Error in gsub(quote, paste0(quote, quote), x, fixed = TRUE) : 
  input string 1 is invalid in this locale

>tbl(mydb,sql("SELECT * from flights"))
>tbl(mydb,sql("SELECT * from cor"))
Error in gsub(quote, paste0(quote, quote), x, fixed = TRUE) : 
  input string 1 is invalid in this locale
{% endhighlight %}

发现如果表名称中包含中文，dplyr会出现[]报错][issue1]。**强烈建议数据库中的表中变量名字尽量不用中文，哪怕用拼音代替也行**

## 3. RSQLite连接数据库

虽然`dplyr`包会报错，但是用`RSQLite`包是没有问题的，只不过多一步转化步骤。详细请看我提交的[RSQLite issue][issue2]

{% highlight r %}
>library(DBI)
>library(RSQLite)
>con <- dbConnect(SQLite(), "E:/SQLite/database/你的数据名字.db")
>query <-"select * from cor;"
>cd <- tbl_df(dbGetQuery(con,query)) 
>cd
Source: local data frame [130,789 x 9]
    XXDM  XBDM  NJDM 韬綋褰㈡€\x81 50绫宠窇绛夌骇 鑲烘椿閲忕瓑绾\xa7
   (chr) (chr) (chr)          (int)          (int)              (int)
1   0015     2    11              3              3                  4
2   0015     1    11              2              2                  4
3   0015     1    11              2              2                  4

> names(cd)<- iconv(names(cd),"UTF-8","GBK")
>cd
Source: local data frame [130,789 x 9]
    XXDM  XBDM  NJDM 身体形态 50米跑等级 肺活量等级 
   (chr) (chr) (chr)    (int)      (int)      (int)        
1   0015     2    11        3          3          4 
2   0015     1    11        2          2          4 
3   0015     1    11        2          2          4 
{% endhighlight %}

RSQLite能够调入数据，但是标题乱码。其原因我个人认为是 sqlite的默认编码是 UTF-8, 而本地R环境是GBK编码，R没有自动将这两个代码转化，故乱码，所以需要加一句函数 `iconv(names(cd),"UTF-8","GBK")`。

请仔细看，我的这个小数据库有13W行，才200M多，很快就能处理完。前天我没有用数据库直接是用dplyr包轻松处理了256W行的数据。一开始还担心会不会爆内存，看来是我多虑了。

## 4. 解决中文乱码问题

中文乱码这个问题困扰我多时，从生成中文PDF -> 中文beamer ->中文分词 ->现在 一直伴随着我，最近摸索了一个套经验，还想了一下原因，仅供参考：

> I think I figure out the reason why Chinese Characters garbled in my R environment. It‘s because the encoding of the data is `UTF-8` while my local environment is `GBK` [(Simplified)_China.936)] , R don't automatically convert the `UTF-8` encoding to `GBK`.  So ,I use `iconv(char,"UTF-8","GBK")` to convert the encoding. ~~装逼，请走开~~。

{% highlight r %}
>library(stringi)
>stri_enc_get()  # 用于检测本地是什么编码环境 
>stri_enc_detect2(char) # 用于检测字符串是什么编码
>stri_conv(char,"UTF-8","GBK") # 直接转码
>iconv(char,"UTF-8","GBK") # 与{base} package 效果一样
{% endhighlight %}

**To solve the problem from the source, I recommend the [{readr}][3] package**

为了从源头解决这个问题，我推荐使用 [readr][3] 这个包。下面的例子仅供参考：

{% highlight r %}
>library(readr)
>guess_encoding("filename.csv")  # guess the encoding of the file

  encoding confidence
1  GB18030          1

>read_csv("filename.csv",col_names = T,col_types = cols(
xxdm = "c"))

Source: local data frame [247 x 6]
    xxdm    nj     economic      chi      mat      eng
   (chr) (int)        (chr)    (dbl)    (dbl)    (dbl)
1   0003    14 <U+00B8><df> 89.93566 85.29368 80.90276
2   0003    15 <U+00B8><df> 83.33544 83.82542 88.48453
3   0004    14 <U+00B8><df> 84.26736 84.38928 75.06619
4   0004    15 <U+00B8><df> 81.33385 84.25302 83.96041
5   0005    14     <d6><d0> 80.89536 81.41362 81.64121
6   0005    15     <d6><d0> 82.39663 82.67030 87.31950

you may find that the `economic` is in the form of UTF-8 encoding.

>read_csv("filename.csv",col_names = T,col_types = cols(
  xxdm = "c"),
  locale = locale(encoding = "GB18030")  # important)

Source: local data frame [247 x 6]
    xxdm    nj economic      chi      mat      eng
   (chr) (int)    (chr)    (dbl)    (dbl)    (dbl)
1   0003    14       高 89.93566 85.29368 80.90276
2   0003    15       高 83.33544 83.82542 88.48453
3   0004    14       高 84.26736 84.38928 75.06619
4   0004    15       高 81.33385 84.25302 83.96041
5   0005    14       中 80.89536 81.41362 81.64121
6   0005    15       中 82.39663 82.67030 87.31950
{% endhighlight %}

**附上我的R环境**

{% highlight R %}
Session info -----------------------------------------------------------------
 setting  value                         
 version  R version 3.2.2 (2015-08-14)  
 system   x86_64, mingw32               
 ui       RStudio (0.99.486)            
 language (EN)                          
 collate  Chinese (Simplified)_China.936
 tz       Asia/Taipei                   
 date     2015-12-01                    
Packages -----------------------------------------------------------------
 package      * version    date       source                            
 assertthat     0.1        2013-12-06 CRAN (R 3.2.1)                    
 DBI          * 0.3.1.9008 2015-11-05 Github (rstats-db/DBI@7a0ad76)    
 devtools       1.9.1      2015-09-11 CRAN (R 3.2.2)                    
 digest         0.6.8      2014-12-31 CRAN (R 3.2.1)                    
 dplyr        * 0.4.3      2015-09-01 CRAN (R 3.2.2)                    
 magrittr       1.5        2014-11-22 CRAN (R 3.2.1)                    
 memoise        0.2.1      2014-04-22 CRAN (R 3.2.1)                    
 nycflights13 * 0.1        2014-07-22 CRAN (R 3.2.2)                    
 R6             2.1.1      2015-08-19 CRAN (R 3.2.2)                    
 Rcpp           0.12.2     2015-11-15 CRAN (R 3.2.2)                    
 RSQLite      * 1.0.9000   2015-11-05 Github (rstats-db/RSQLite@1187d0c) 
{% endhighlight %}

### 5. 最后 
改手机界面确实好难，一点都不懂CSS+html的人，完全无从下手。上个周末改版失败，还没有这个好看，所以暂时就这样了。所以为了更好的观看体验，请使用网页端。


[issue1]: https://github.com/hadley/dplyr/issues/1561
[issue2]: https://github.com/rstats-db/RSQLite/issues/108
[3]: https://github.com/hadley/readr
