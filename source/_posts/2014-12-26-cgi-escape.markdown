---
layout: post
title: "Ruby 下載檔案時遇到中文字"
date: 2014-12-26 08:48:00 +0800
comments: true
categories: ["ruby"]
---

在做下載檔案的功能時，如果遇到中文字的檔名`Ruby`就不認得了，是可以下載，不過檔名就不是你原先設計的那個檔名。

所以只要加上 CGI.escape 將他轉成URL可以看的懂的編碼就行了

	
	CGI.escape("我是中文字.csv")
	