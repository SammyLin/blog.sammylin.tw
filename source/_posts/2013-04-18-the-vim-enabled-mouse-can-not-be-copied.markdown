---
layout: post
title: "[VIM]啟用滑鼠時無法複製"
date: 2013-04-18 21:38
comments: true
categories: [vim]
---

在`vimrc`加入下面這一行時

    :set mouse=a

發現用滑鼠時候會轉成Visual模式，但是按control+c就是不能複製
Google 查了一下，按發現其實有解法的

#### 在OS X下，按住`option`，用滑鼠點選 

#### 在其他系統下，按住`shift`，用滑鼠點選

