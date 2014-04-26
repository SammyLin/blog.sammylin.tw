---
layout: post
title: "Octopress統計文章數量"
date: 2012-01-19 10:19
comments: true
categories: [Octopress]
---

最近在訂定一個小目標，就是要在今年破200篇文章，但是octopress的外掛需要自己寫，看了高見龍大大的[幫你的Octopress增加文章分類](http://blog.eddie.com.tw/2011/12/05/add-catetories-to-sidebar-in-octopress/)文章，也直接偷偷改了他的程式碼，產生文章數量的silderbar。

把下面的程式碼新增到`plugins/article_total.rb`
<!-- more -->
{% gist 1637296 %}

這邊是會將你的outopress建立的post的數量，建立一個html的檔案

然後把下面幾行設定檔加到`_config.yml`

```
# ---------------------- #
#  Article Total         #
# ---------------------- #
#
# create article total page
article_total: true
# article total slider title
article_total_title: Article Total
# article total slider text
article_my_site_has: My site has
article_my_site_article : articles
# create an include article total page in @source/_includes/asides/article_total_sidebar.html
article_total_sidebar true
```
可以修改的地方是`article_my_site_has`跟`article_my_site_article`按照你的語言
如果是要中文的話可以把這兩行改成

```
article_my_site_has: 本站共有
article_my_site_article : 篇文章
```

最後就要把產生的`article_total_sidebar.html`檔案加到`_config.yml`

在default_asides:裡面加上`asides/article_total_sidebar.html`

才能夠將article total 加到 asides裡面!

因為我也是改[高見龍](http://blog.eddie.com.tw/)大大的程式，如果有問題可以跟我說，或是有更好的建議可以提供給我~~

#### 參考網站

> [幫你的Octopress增加文章分類](http://blog.eddie.com.tw/2011/12/05/add-catetories-to-sidebar-in-octopress/) by 高見龍

