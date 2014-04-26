---
layout: post
title: "[Ruby]在model生成Schema information-annotate"
date: 2012-01-18 11:17
comments: true
categories: [annotate, Ruby on Rails]
---

`annotate`也是一個好用的工具，像我過去要看我table的schema時，還要開mysql GUI tools 來看，不然的話我都忘記我的schema是什麼(初老症越來越嚴重了)，只要安裝`annotate`的話他會幫你自動產生Schema information 在你的model裡面，就不用一直去看你的table裡的資料，這樣子的話coding的速度就會加快許多。

<!-- more -->
### 安裝annotate

在你的Gemfile裡面加上

	gem 'annotate', :git => 'git://github.com/ctran/annotate_models.git'

bundle install
	$ bundle install

### 產生schema information

	$ bundle exec annotate
這樣他就會乖乖的產生出來在你的model最下面

	# == Schema Information
	#
	# Table name: articles
	#
	#  id                         :integer(4)      not null, primary key
	#  Title                      :string(255)
	#  Content                    :text

### 產生routes

上次有講怎麼在[產生Rails專案裡所有的routes](http://blog.igotcloud.com/rails-rake-routes/)利用`annotate`的話也可以產生routes的對應資料放到你的routes.rb裡面

	$ bundle exec annotate -r

#### 參考網站

> [ctran / annotate_models](https://github.com/ctran/annotate_models) by Github
