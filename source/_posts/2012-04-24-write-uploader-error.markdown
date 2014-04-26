---
layout: post
title: "出現private method `write_uploader' called for"
date: 2012-04-24 22:29
comments: true
categories: [Ruby on Rails]
---

用[carrierwave](https://github.com/jnicklas/carrierwave)做我的upload plugin
用的時候很正常，有一天突然噴出了錯誤訊息

	NoMethodError in Admin::ArticlesController#update

	private method `write_uploader' called for #<Article:0x007f8f6cd1c3d0>

解了老半天也上網google了一下，也到Github上找[issue](https://github.com/jnicklas/carrierwave/issues/511)，都沒辦法！

最後才發現


{% gist 2485397 %}
 
 我有一個`scope`叫做`public`，經過[小小的了解](http://stackoverflow.com/questions/7658359/what-is-objectprivate-and-objectpublic-in-ruby)發現雖然不是不是保留字，不過是拿來當修飾詞的，雖然不是每一個method都會用到，不過拿來用可能會有問題！
 
 所以只要把`scope :public`改成其他的字就行了
