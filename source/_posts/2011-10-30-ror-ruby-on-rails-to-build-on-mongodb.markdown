---
layout: post
title: ! '[Rails]Ruby on Rails上建置MongoDB'
categories: [Ruby on Rails, DataBase]
---
慢慢開始要繼續Coding了，最近碰到Nosql，想說要在Ruby on Rails上面開發Nosql來玩玩，所以選擇了MongoDB。MongoDB是文件導向的資料庫，是由C++開發的，而且官方開發了許多的Drive，就連我過去使用的C#也包含在裡面，這真是吃人夠夠呀(誤)，MongoDB也有gui的資料庫管理-MongoHub 只是我在寫這篇文章的同時，他的網站好像掛了，如果要下載可以到OSX玩意下載玩玩。
### 1.建立Rails框架
	$ rails new mongo_test
### 2.編輯GemFile，新增下面幾行

``` ruby GemFile
source 'http://gemcutter.org'
gem "mongo_mapper"
gem 'rails', '3.1.1'
gem 'bson_ext'
```
<!--more-->

### 3.新增一個檔案 config/initializers/mongo.rb 來指定資料庫的設定
``` ruby config/initializers/mongo.rb
#指定資料庫的位址跟port號
MongoMapper.connection = Mongo::Connection.new('localhost', 27017)
#資料庫的名稱
MongoMapper.database = "#myapp-#{Rails.env}"


if defined?(PhusionPassenger)<br />
   PhusionPassenger.on_event(:starting_worker_process) do |forked|
     MongoMapper.connection.connect if forked
   end
end
```
### 4.編輯config/application.rb
``` ruby config/application.rb
require "action_controller/railtie"
require "action_mailer/railtie"
require "active_resource/railtie"
require "rails/test_unit/railtie"
#require 'rails/all'
```
### 5.執行bundle install
	$ bundle install
### 6.啟動rails server
	$ rails server

你會看到少了下面這兩行訊息，就表示成功了
Database adapter sqlite3
Database schema versionp

### 7.建立一個Scaffold
`注意：要加上--orm=mongo_mapper`

	$ rails generate scaffold board name:string --orm=mongo_mapper
### 8.直接開瀏覽器
[http://127.0.0.1:3000/boards](http://127.0.0.1:3000/boards)

![](http://blog-img.igotcloud.com/wp-content/uploads/2011/10/NewImage.png)

![](http://blog-img.igotcloud.com/wp-content/uploads/2011/10/NewImage1.png)

![](http://blog-img.igotcloud.com/wp-content/uploads/2011/10/NewImage2.png)