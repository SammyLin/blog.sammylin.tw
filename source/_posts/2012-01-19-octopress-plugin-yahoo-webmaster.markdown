---
layout: post
title: "在你的octopress裝上yahoo站長工具"
date: 2012-01-19 14:25
comments: true
categories: [Octopress, yahoo webmaster]
---

很可惜，Octopress沒有Yahoo站長工具，只有`Google Analytics`所以這樣子的話只好自己裝了，因為每個人有每個不同的習慣，有的愛看Google有的愛看Yahoo，像我有時候會看兩個都看，其實我也不知道要看什麼，看那個人數飆高就有一股爽度，那是一開始啦，到最後已經沒有那種爽度了，我還記得以前每天都會去看，搞的好像看股票一樣，加入Yahoo站長工具，其實不難，基本上只要看的懂HTML，然後把他加進去就好了，不過樣子還蠻不漂亮的，我就把他做成一個plugin，供大家使用囉。

<!-- more -->

建立一個檔案`source/_includes/custom/yahoo_webmaster.html`把下面這些丟進去
{% gist 1638433 %}

然後開啟`_config.yml`加入以下的程式碼

	# Yahoo Webmaster
	yahoo_webmaster_id: <id>

如果不知道的話可以進到[Yahoo站長工具](http://tw.webmaster.yahoo.com/stats/)建立你的網站，進到詳細資料，他的網址最後面就會有一個參數叫unit_id，後面就是你的ID了

最後就是要把你的站長工具放入你的頁面，因為考慮到有些人可能會開啟`統計貼紙`所以我把他放在`aside`裡面

所以只要在`_config.yml`找到default_asides:後面加上`custom/yahoo_webmaster.html`就完成啦


