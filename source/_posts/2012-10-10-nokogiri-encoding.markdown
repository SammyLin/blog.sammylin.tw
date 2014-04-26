---
layout: post
title: "[Ruby]編碼學問很大"
date: 2012-10-10 11:56
comments: true
categories: [Ruby]
---

### 問題：
[Nokogiri](http://nokogiri.org)是一個網頁擷取的一個很好的工具，所以我會用他當做我要抓網站資料的首選，就像Nokogiri的網站使用說明一樣，我們只要用下面三行就可以使用Nokogiri，之後在用他內建幾個method來做處理，但今天不講怎麼做處理

``` ruby
require 'nokogiri'
require 'open-uri'
doc = Nokogiri::HTML(open('http://www.google.com/search?q=sparklemotion'))
```
<!-- more -->

我今天在抓一個舊網站的資料，因為以前網站在製作的時候都是用`Big5`所以要抓資料的時候會用`Iconv`來轉碼轉成`utf-8`就像下面一樣

``` ruby
ic = Iconv.new("utf-8","big5")
doc = Nokogiri::HTML(ic.iconv(open('http://www.missingkids.org.tw/chinese/focus.php?offset=0').read))
```
不過噴出了錯誤

	encoding error : input conversion failed due to input error
	
找了一段時間常發現問題出在全型的`/`，我猜想是因為Big5轉到utf-8的時候，因為全形的`/`編碼有問題，可能是不能Big5或者是utf-8裡的全形`/`跟Big5的編碼不一樣，我也不是很清楚

### 解決方法
其實解決的方法其中一個方法就是....跳過他(真不負責任XDD)
就像這樣

``` ruby
ic = Iconv.new("utf-8//IGNORE","big5")
```

`//IGNORE`就是遇到轉碼有問題的話，直接跳過這個字，還有另外一個`//translit`，如果遇到有問題的話，就會去找替代的字，如果不行的話還是會中斷，但中文字經常會找不到對應的編碼。不過可以混合使用，就像這樣

``` ruby
ic = Iconv.new("utf-8//translit//IGNORE","big5")
```

