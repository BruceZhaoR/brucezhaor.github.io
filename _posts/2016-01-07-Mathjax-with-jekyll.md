---
layout: post
title: MathJax with Jekyll
subtitle: "在jekyll中使用MathJax生成漂亮公式"
category: Jekyll
tags: [MathJax]
mathjax:    true
date:       2016-01-07
author:     "Bruce Zhao"
header-img: "/img/post/mathjax-logo.svg"
description:  "看到许多博客里面的公式都是图片截图，在小赵眼里觉得很low。于是乎，小赵就开始寻找在jekyll中生成漂亮公式的方法- MathJax"
---

## 写在前面的话

我最开始接触在html中写$$LaTeX$$公式是在使用Rmarkdown的时候，因为Rmarkdown只需要knit一下就能将markdown生成html。特别是公式只需要在公式两端加上`$公式$`(inline) `$$ 公式 $$`(equation)就能生成公式，特别对学统计学的小赵来说，特别的方便(在熟练的LaTeX公式的情况下)，再也不用word公式编辑器里面去拖拉拽了。

自从接触了jekyll以后，就疯狂的爱上了这个工具，渐渐的Rmarkdown用的少了。现在使用Rmarkdown主要就是生成PDF和beamer报告用，以及帮帮研究僧同学调一调PDF格式用于提交作业。

之所以弃用Rmarkdown生成html的原因是不会调里面的CSS布局，以及增加手机阅读的适配性(现在几乎都是手机阅读，所以手机端的阅读体验是第一位)。而jekyll结合了bootstrap后，简直炫酷到不行，关键是有很多大牛的写好的现成的模板，Chorme -> <kbd>F12</kbd> 就能扒到css和版式。或者直接找到对应的github repository fork下来或者干脆download Zip，你就拥有了别人的博客了。多扒几个，研究研究就能融会贯通了，然后做成你想要的博客模板。

感觉说了很多废话啊，还没有进入主题，可能是太久没有写一点东西了(还欠着2015年的总结啊，多少人盼望着...)，最近一直忙着改版博客，现在已经是第五次改版了重要满意了。

## 在jekyll中调用MathJax JavaScript

在jekyll中调用$$MathJax$$ <https://www.mathjax.org> 的JavaScript就能实现LaTeX公式渲染。我测试的markdown渲染工具是kramdown,Github Flavored Markdown。

### 首先

你需要将一段script加入到_layout/post.html 里面。我是不会告诉你这段代码是从knited的html里面copy过来的(在最后面)。

{% highlight javascript %}

// mathjax 
<script>
  (function () {
    var script = document.createElement("script");
    script.type = "text/javascript";
    script.src  = "https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML";
    document.getElementsByTagName("head")[0].appendChild(script);
  })();
</script>

{% endhighlight %}


### 其次

需要说明的是这里引发渲染的与Rmarkdown里面的略有区别：`$公式$`在这个里面是不能用的; `$$ 公式 $$` 在Rmarkdown里面是居中的公式，而在这里是Inline公式；这里的居中另起一行的是 `\\[ 公式 \\]`；也可以用 `\\( 公式 \\)`来实现Inline公式。

### 示例

这是原始LaTeX公式语法：

```
$$ \int_a^b f(x)\,dx $$ #Inline公式

\\[\int_0^{+\infty} x^n e^{-x} \,dx = n! \\]  另起一行居中公式

\\( p\{X=x\}=f(x)=\frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}} \\)  Inline公式

\\[ p\{X=x\}=f(x)=\frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}} \\]  另起一行居中公式

$$ F(x) = P\{ X <= x \} = \int_{-\infty}^x f(x)\,dx $$

\\[ f(x,y,z) = 3y^2 z \left( 3 + \frac{7x+5}{1 + y^2} \right). \\]
```


这是对应生成的公式样子：

$$ \int_a^b f(x)\,dx $$ #Inline公式

\\[\int_0^{+\infty} x^n e^{-x} \,dx = n! \\] #另起一行居中公式

\\( p\{X=x\}=f(x)=\frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}} \\) #Inline公式

\\[ p\{X=x\}=f(x)=\frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}} \\] #另起一行居中公式

$$ F(x) = P\{ X <= x \} = \int_{-\infty}^x f(x)dx $$

\\[ f(x,y,z) = 3y^2 z \left( 3 + \frac{7x+5}{1 + y^2} \right). \\]

### **炫酷吗** 右键选择 Math Settings -> zoom trigger -> click 点击就能放大公式了。

## 最后

欢迎大家在下面的多说评论框拍砖以及分享，支持各种主流社交账号登陆。虽然界面相比之前的disqus丑了一点，但是毕竟在国内，你懂的。想知道disqus的界面的同学请点击  <a target="_blank" href="http://brucezhaor.github.io/about.html">这里</a>


