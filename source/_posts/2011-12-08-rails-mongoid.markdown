---
layout: post
title: "[rails]建置MongoDB環境利用mongoid"
date: 2011-12-08 14:18
comments: true
categories: [Ruby on Rails , mongodb , mongoid]
---
之前用了`mongo_mapper`感覺很好用，不過發現要裝一些plugin的時候，像是desice時，就會有一些問題的存在，所以我改用`mongoid`來使用，比較順許多，而且不用在`rails g model XXX --orm==mongo_mapper`之類的。

### 用gem 安裝
``` ruby Gemfile
gem "mongoid", "~> 2.3"
gem "bson_ext", "~> 1.4"
gem 'devise'
```
`$ bundle install`

### 產生資料庫設定檔

`$ rails g mongoid:config`
``` ruby config/mongoid.yml
development:
  host: localhost
  database: demo3_development

test:
  host: localhost
  database: demo3_test

# set these environment variables on your prod server
production:
  host: <%= ENV['MONGOID_HOST'] %>
  port: <%= ENV['MONGOID_PORT'] %>
  username: <%= ENV['MONGOID_USERNAME'] %>
  password: <%= ENV['MONGOID_PASSWORD'] %>
  database: <%= ENV['MONGOID_DATABASE'] %>
  # slaves:
  #   - host: slave1.local
  #     port: 27018
  #   - host: slave2.local
  #     port: 27019
```
<!--more-->
### 修改 config/application.rb
`vim /config/application.rb`
``` ruby config/application.rb
require "action_controller/railtie"
require "action_mailer/railtie"
require "active_resource/railtie"
require "rails/test_unit/railtie"
#把rails/all前面加上#
#require 'rails/all'
```
### 產生devise設定檔
`$ rails g devise:install`

```
      create  config/initializers/devise.rb
      create  config/locales/devise.en.yml

===============================================================================

Some setup you must do manually if you haven't yet:

  1. Setup default url options for your specific environment. Here is an
     example of development environment:

       config.action_mailer.default_url_options = { :host => 'localhost:3000' }

     This is a required Rails configuration. In production it must be the
     actual host of your application

  2. Ensure you have defined root_url to *something* in your config/routes.rb.
     For example:

       root :to => "home#index"

  3. Ensure you have flash messages in app/views/layouts/application.html.erb.
     For example:

       <p class="notice"><%= notice %></p>
       <p class="alert"><%= alert %></p>

  4. If you are deploying Rails 3.1 on Heroku, you may want to set:

       config.assets.initialize_on_precompile = false

     On config/application.rb forcing your application to not access the DB
     or load models when precompiling your assets.

===============================================================================
```
`$ rails g devise User`

### 建立一個鷹架

`$ rails g scaffold board name:string`

### 修改加入root :to

`vim config/routes.rb`
``` ruby config/routes/rb
root :to => "boards#index"
```

### 刪除預設首頁

`rm public/index.html`

### 增加使用者管理至主版頁面
`vim app/views/layouts/application.html.erb`

將下面這個介面放在`<%= yield %>`上面

``` ruby app/views/layouts/application.html.erb
<% if user_signed_in? %>
<ul>
 <li>
  <%= link_to('Edit registration', edit_user_registration_path) %>
  </li>
  <li>
  <%= link_to('Logout', destroy_user_session_path, :method => :delete) %>        
  </li>
</ul>
<% else %>
<ul>
  <li>
  <%= link_to('Login', new_user_session_path)  %>  
  </li>
  <li>
  <%= link_to('Register', new_user_registration_path)  %>
  </li>
</ul>
<% end %>
```
### 預覽
`$ rails s`

打開溜覽器進入[http://127.0.0.1:3000](http://127.0.0.1:3000)

你就會看到以下畫面
![](https://lh3.googleusercontent.com/-zP1qQNfkY5Q/TuCGA8dTZlI/AAAAAAAABfI/4nl52LlLPto/s400/Screen%252520Shot%2525202011-12-08%252520at%252520%2525E4%2525B8%25258B%2525E5%25258D%2525885.38.25.jpg)

#### 註冊
![](https://lh3.googleusercontent.com/-vll9mK2RRoE/TuCHapM0tqI/AAAAAAAABfg/2lm6PW69znI/s640/Screen%252520Shot%2525202011-12-08%252520at%252520%2525E4%2525B8%25258B%2525E5%25258D%2525885.43.19.jpg)

#### 登入後畫面
![](https://lh6.googleusercontent.com/-Oes4BaXlwv8/TuCHahKrp2I/AAAAAAAABfc/r4sEp5tThyc/s640/Screen%252520Shot%2525202011-12-08%252520at%252520%2525E4%2525B8%25258B%2525E5%25258D%2525885.43.38.jpg)

#### 登入畫面
![](https://lh6.googleusercontent.com/-VX9XHmio9_4/TuCHbcjU8lI/AAAAAAAABfk/8VMGykwSiFU/s640/Screen%252520Shot%2525202011-12-08%252520at%252520%2525E4%2525B8%25258B%2525E5%25258D%2525885.43.53.jpg)

#### 修改個人資料
![](https://lh5.googleusercontent.com/-ReZup33N5hA/TuCHavp2E6I/AAAAAAAABfo/2Zvbv361zrA/s640/Screen%252520Shot%2525202011-12-08%252520at%252520%2525E4%2525B8%25258B%2525E5%25258D%2525885.44.02.jpg)
