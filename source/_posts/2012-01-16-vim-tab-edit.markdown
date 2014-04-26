---
layout: post
title: "在Vim裡面做分頁"
date: 2012-01-16 14:36
comments: true
categories: [vim, tab]
---
今天又學到一招好用的東西，就是在`vim`裡面做Tab(分頁)，用法很簡單。

### 開啟第一個Tab

	:tabedit <filename>

or

	:tabe <filename>

<!-- more -->

### 切換下一個Tab
	:tabnext

or

	:tabn

or 

	gt

### 切換上一個Tab

	:tabNext

or

	:tabN

or

	gT

### 切換到第一個Tab

	:tabrewind

or 

	:tabr

### 切換到最後一個Tab

	:tablast

or

	:tabl

### 顯示所有的Tab

	:tabs

### 用vim開啟多個檔案

	$ vim -p <filename1> <filename2> <filename3>
 
#### 參考網站

> [vim 的小玩意：tab 怎麼玩？](http://nicaliu.pixnet.net/blog/post/18589570) at 尼卡 Nica

> [vim tab 功能](http://navyblueshellingford.blogspot.com/2008/02/vim-tab.html) at shelling

