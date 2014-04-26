---
layout: post
title: "我的Git筆記"
date: 2012-01-12 13:01
comments: true
categories: [Git]
---

自從看了XDite的[Rails 101 - 火速學會 Ruby on Rails](http://rails-101.logdown.com/)之後，就買了第一台的MAC，發現有好多東西需要學習，就像Git也是我近期才學會的，可能會很少資料，不過慢慢用慢慢的補充資料

### 第一個GIT

`$ mkdir project`

`$ cd project`

`$ git init`

會建立一個.git的目錄，裡面放的是git repository的檔案。

`$ git add . `

把新增跟修改得資料放到staging area裡面。不包含刪除的資料。

`$ git commit ` or `$ git commit -m 'my first project'`

提交這次的改變的資料，可以直接git commit 或是在後面加上-m 寫上這次提交的訊息

<!-- more -->
### 刪除檔案

在Git裡面如果需要刪除某個檔案的話，不然的話commit時，他會記錄實體檔案被刪除，但是不會在repo裡做動作，建議是使用

`$ git rm filename`

但是如果已經被刪除了，或是不想要讓這個檔案被commit的話，可以用

`$ git rm --cache filename`

當commit的時候，就會把資料清除掉

### 回到過去

ㄚ..這次的東西做爛了，有時候精神不濟，想東想西，所以這次東西做爛了，但我還沒有commit這時候就只好回到過去了。

`$ git add .`

`$ git reset --hard HEAD`

或是

`$ git checkout -f`

如果你剛好是用ruby的話，已經做過`rake db:migrate`的話，表示你資料庫做過migrate，而你要回到之前的狀態，你可以使用

`rake db:rollback`

你要回到前兩次的話

`rake db:rollback STEP=2`

### clone

這台電腦第一次的做這個專案時要把檔案抓下來，或是你在[github](https://github.com/)看到不錯的專案，或是要幫別人修issues，但是能幫別人修的話，應該不會看這篇啦= =

可以直接打

`$ git clone http://git.example.com/project.git` 

### 忽略檔案、目錄

在專案目錄下

`$ vim .gitignore`

把你要忽略的檔案錄目錄加到最後一行去就行了

參考網站

> [Git 初學筆記 - 實作測試](http://blog.longwin.com.tw/2009/05/git-learn-test-command-2009/) at Tsung's Blog

> [Git 初學筆記 - 指令操作教學](http://blog.longwin.com.tw/2009/05/git-learn-initial-command-2009/) at Tsung's Blog

> [寫給大家的git教學](http://www.slideshare.net/littlebtc/git-5528339)