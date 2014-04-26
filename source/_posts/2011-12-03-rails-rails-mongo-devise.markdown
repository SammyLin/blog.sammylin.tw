---
layout: post
title: "[Rails]安裝devise在mongo_mapper"
date: 2011-12-03 00:03
comments: true
categories: [Ruby on Rails]
---
參考了[XDite](http://blog.xdite.net/)的`Rails 101`慢慢的上手了，而我希望可以配合`MongoDB`來建立我的網站不過還有些地方跟`Rails 101`不大一樣，所以來筆記一下了

使用者機制在`Rails`很快就可以完成，因為我們可以直接用別人開發過的Plugin，就可以懶懶的完成你所要的功能了，在`Rails 101`上面推薦的使用者Plugin是`Devise`
<!--more-->

### 修改Gemfile

``` ruby Gemfile
gem 'devise', '1.1.3'
gem 'devise-mongo_mapper',
  :git    => 'git://github.com/collectiveidea/devise-mongo_mapper'
```

### 安裝新的plugin

`$ bundle install`

### 產生Devise檔案

`$ rails generate devise:install`

### 修改config/initializers/devise.rb 配置

``` ruby config/initializers/devise.rb
#在後面加上mongo_mapper
require 'devise/orm/mongo_mapper'
```
### 建立users的model

`rails generate devise users`

其他的就沒什麼差別了~
