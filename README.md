## 重新用hugo构建博客准备工作

### 页面布局

1. page 链接

```css
a {
  color: #f05948;
  text-decoration: none;
}
article a:hover {
  font-weight: 700;
  color: #00b0cf;
}
article a:visited {
  color: #ccc;
}

::selection {
    background: #0ac478;
    color: white;
    }
    
::-moz-selection {
    background: #0ac478;
    color: white;
    }
    
body {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, "PingFang SC","Microsoft YaHei","Microsoft JhengHei","Source Han Sans SC","Noto Sans CJK SC","Source Han Sans CN","Noto Sans SC","Source Han Sans TC","Noto Sans CJK TC","WenQuanYi Micro Hei", SimSun, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
}

```
2. 站内搜索功能

- RStudio的keras用的是`swiftype`
- cosx.org用的是Algolia instantsearch.js
- lunr.js 之前用过，中文：http://linfuyan.com/add-chinese-support-to-lunrjs 
- hugo-algolia https://github.com/10Dimensional/hugo-algolia 
- pkgdown 采用 algolia.


3. 代码高亮

- highlight.js + nord css https://github.com/arcticicestudio/nord-highlightjs 
- 字体采用 dejavu Sans mono

4. 评论系统

- gitment  https://github.com/imsun/gitment
- http://mazhuang.org/about/ 

5. 细节优化

- 主页的navbar，红条 <https://themes.gohugo.io/theme/minimal/>
```css
:root {
    --accent: red;
    --border-width: 5px;
}

.navbar.navbar-default {
    border-top: var(--border-width) solid var(--accent);
}
.navbar-default {
    background-color: #f8f8f8;
    border-color: #e7e7e7;
}
.navbar-fixed-top {
    top: 0;
    border-width: 0 0 1px;
}

footer {
    border-bottom: var(--border-width) solid var(--accent);
}

```

