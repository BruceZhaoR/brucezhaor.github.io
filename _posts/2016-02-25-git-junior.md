---
layout: post
title: "Git进阶"
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

## 分支的创建(why)

### gh-pages

通过创建分支，并且命名为`gh-pages`就能生成网站，用于项目展示，或者当做另外一个博客来用。在settings里面可以快速设置项目主页，通过编辑markdown，github直接给你生成一个index.html网页，然后会出现访问的地址。

### 开发新的特性

你突然有了一个新的想法，但是不知道这个新的想法是否可行，你可以创建分支，将目前master里面的内容copy到分支中，然后在此基础上进行开发，而不会影响到主分支master。等开发测试通过后，merge到master中，就能实现新功能的添加。如果不可行，也可以删除分支，总之是不会影响到master的。

### 实例与常用命令

假设你已经clone了master分支到了本地，现在要创建一个分支用作项目展示的网站。这个时候你需要创建一个`gh-pages`的分支。下面的代码就是创建分支的过程。Ps:作为网站的话，这个分支下面需要有`index.html`文件,然后你点击`settings`就能看到自动生成的网站地址。

{% highlight shell %}

git checkout -b gh-pages	#新建分支,名为gh-pages,并switch.
git branch -vv			#查看分支信息
git remote -v			#查看远程仓库信息
git push origin gh-pages	#将本地新建的分支同步到github上面

{% endhighlight %}

假设你需要创建一个名为`new_branch_name`的分支来开发新的特性。

{% highlight bash %}

git branch new_branch_name	#来创建新的分支
git checkout new_branch_name	#switch到分支
git branch 			#查看分支列表
git checkout master 		#switch到主干/master

{% endhighlight %}



## 分支的操作

创建完分支后，需要对分支进行操作与管理，下面简单的举几个例子来帮助大家理解。

### 重命名分支

{% highlight bash %}

git checkout -b test 		#创建新的分支并switch
git branch     			#查看分支状态
git branch -m dev		#直接将test重命名为dev
git branch			#查看分支状态

{% endhighlight %}

### 删除分支

*删除分支的时候需要切换到其他分支，然后才能删除当前的分支。*

{% highlight bash %}

git checkout master			#切换到主分支
git branch -D dev			#删除dev分支

{% endhighlight %}

如果分支之前已经同步到github上面，那你需要删除掉远程对应的分支。
`git push origin :dev` 删除远程github上面的dev分支。当然，这样的操作可以直接在github上面通过点击来完成。

## 分支的同步与合并

有时候自己在公司修改了github上面的文件，或者organisation里面其他成员更新了内容到repository里面了，你回到家中想继续工作，这个时候你需要把更新后的repository同步到本地来。这样你在本地修改后再同步到github上面时就不会发生冲突了。关于合并碰到冲突，我目前还没有碰到，以后碰到的话总结一下，再写一篇。

### 自己repository的同步

还是来一个例子吧：

{%highlight bash %}

git checkout gh-pages		#切换到需要同步的分支
git branch -vv 			#查看分支信息
git pull 			#自动同步过来
git branch -vv			#会发现commit变成最新的了

{% endhighlight %}

同步完成后，你就可以在家里的电脑进行各种操作了。当操作完成后，你需要将工作同步到github，这样你第二天同步到公司的电脑了。

{% highlight bash %}

git add .			#添加所有修改过的文件
git commit -m "修改注释" 	#添加修改注释
git push 			#同步到远程github

{% endhighlight %}

这样你就完成了一个类似`云端`的操作，随时随地都能做开发，写东西 :)

### 版本回退

有时候开发做着做着发现不太对，我要回到出错前的那个状态该肿么办?好在git有类似时光机的功能。

{% highlight bash %}

git log -5			#显示最近5条commit记录
git reset --hard HEAD^		#回退1个commit
git reset --hard HEAD^^		#回退2个commit
git reset --hard 3628164	#回退指定的commit

{% endhighlight %}

每次commit之后，git都会生成很长的一串SHA值，但是有用的就前7位。通过`git log`来查看提交历史记录的commit，按<kbd>Enter</kbd>查看更多记录，当然高级的方法是检索匹配，[git-pro-2.3](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%9F%A5%E7%9C%8B%E6%8F%90%E4%BA%A4%E5%8E%86%E5%8F%B2) 这本书有详细说明。目前我还没有用到，以后用到了再写。

找到相应的commit后，按<kbd>Ctr + C</kbd>终止命令。然后`git reset --hard <前7>` 就能回退到相应的版本了。所以，你在每次提交commit的时候是不是应该写清楚你干了什么呢?

### fork后的同步

作为菜鸟，我们经常性的fork别人的repository,但是fork了别人的repository之后怎么同步呢？下面我就来讲讲通过git操作的方法。如果是github的话 -> `new pull request`,以自己的repository为base fork，以fork的repository为headfork，然后提交pull request，就能实现更新了。

比如我fork了`qinwf/awesome-R`这个repository，这个经常更新我需要去同步过来。

{% highlight bash %}
git remote add qinwf-awr git@github.com:qinwf/awesome-R.git
git remote -v						#会发现多了一个qinwf-awr
git branch -u qinwf-awr/master master 			#为master添加上游仓库 
git branch -vv
git pull 						#完成同步
git push origin master					#同步到自己的远程github仓库
{% endhighlight %}

添加了upstream repository后，今后只需要`git pull`然后就能同步了，但是同步到自己的远程仓库是需要指定名称，例如`origin`。

另外还有其他的办法：

{% highlight bash %}
git fetch qinwf-awr	master
git checkout master 			#如果你本来就是master就不用switch
git merge qinwf-awr/master		#合并同步
#或者
git checkout -b qinwf-awr 		#新建分支
git pull git://github.com/qinwf/awesome-R.git master
git checkout master
git merge qinwf-awr			#合并同步
git push origin master --force		#强制覆盖远程仓库
{% endhighlight %}

还有另外一种方法: `git branch --track iss1 origin/master`  #创建一个新的分支 iss1 用于track 远程/上游仓库,然后你就可以跟踪远程的更新，而不影响现在分支的开发。

当你不想follow更新了，你可以删去上游仓库: `git remote rm qinwf-awr` .

当你看到了这里时，恭喜你达到了中阶1阶段的水平。掌握了这些命令，就能够处理绝大多数的git/github问题了 :)

## Resources

- [git/github guide -- a minimal tutorial](http://kbroman.org/github_tutorial/)
- [快速上手](http://rogerdudler.github.io/git-guide/index.zh.html)
- [cnblogs](http://www.cnblogs.com/fengyv/archive/2014/06/16/3791588.html).[看日记学git](http://roclinux.cn/?p=213)
- [廖雪峰git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)。
- [fork后的同步](https://github.com/hadley/ggplot2/wiki/Developing-ggplot2-using-github)
- [git-pro](https://git-scm.com/book/zh/v2)
