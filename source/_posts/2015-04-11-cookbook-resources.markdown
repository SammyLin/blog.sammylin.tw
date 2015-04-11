---
layout: post
title: "[Chef] Cookbook 寫 Recipe 常用的 Resource 介紹"
date: 2015-04-11 14:51:43 +0800
comments: true
categories: chef
---

Chef 有多重要，有多好那我們就不說了，如果你只是路過想了解什麼是 Chef 那你可以參考下面這個連結：

[[Rails佈署實戰教學]使用Chef-Solo一鍵安裝機器 - 好麻煩部落格](http://gogojimmy.net/2013/06/01/Chef-Solo-Basic-with-Vagrant/)

**這篇重點放在幾個常用的 Resource (我覺得常用的 XD)，如果有大大覺得哪個也是重要的那也可以一起討論或分享一下。若是文章有錯誤的地方也煩請指教**

如果你已經開始在製作一個 cookbook 時，在寫一個 recipe 常常會不知道要用什麼 resource ，或是不知道有什麼可以用。而且我看英文的速度沒辦法一目十行呀，而且中文的資料找不到，所以只能自已做個筆記來提醒我自已。也可以直接到 [Resources Reference — Chef Docs](https://docs.chef.io/resources.html) 參考官方網站的資料(當然那邊是最齊全的)，

先簡單了解一下一個 cookbook 的資料夾架構

```

├── attributes     (給 recipe 使用的預設值
├── files          (需要傳入到節點的檔案
│   └── default
├── metadata.rb    (描述 cookbook 
├── recipes        (這個 cookbook 的食譜的做法，這篇就是要講怎麼做 (不知道怎麼翻XD
│   └── default.rb
└── templates      (透過 ERB template 可以產出檔案到節點
    └── default
```

所以我們會在 recipes 中建立這個 cookbook 要做的事情有什麼，所以要利用 chef 的 resources 寫這個 recipe。 

<!-- more -->


### 套件 Package

```
package 'nginx' do
  action :install     # 動作 (如果是 :install 可省略
  version "<version>" # 版本 (如不指定就安裝最新的
end

```

`:action` 的方法屬性

| Action    | Description                                                |
| --------- | ---------------------------------------------------------  |
| :install  | **(預設)** 安裝                                             |
| :purge    | 移除 package (只有 Debian 上可以用，其他平台還是用 `:remove`)   |
| :reconfig | 配合 `response file` 重新設定                                |
| :remove    | 移除 package                                               |
| :upgrade   | 升級 package 到最新版                                       |



### 目錄 Directory

```
directory "<path>" do
  owner "root"   # 資料夾擁有者
  group "root"   # 資料夾群組
  mode "0755"    # 檔案權限
  action :create # 動作 (如果是 :create 可省略
end
```

`:action` 的方法屬性

| Action    | Description                                             |
| --------- | ------------------------------------------------------  |
| :create   | **(預設)** 建立目錄(但是如果設定不一樣還是會處理到符合設定)   |
| :delete   | 刪除目錄                                                 |


### 服務 Service

```
service "<service name>" do
  action :start # 動作 
end
```

`:action` 的方法屬性

| Action   | Description                |
| -------- | -------------------------- |
| :disable | 關閉開機時並開啟服務          |
| :enable  | 啟用開機時並開啟服務          |
| :nothing | **(預設)** 不做任何事          |
| :reload  | 重新載入 configuration 配置  |
| :restart | 重啟服務                     |
| :start   | 開啟服務                     |
| :stop    | 停止服務                     |

`:supports` 的方法屬性說明

預設是 `{ :restart => false, :reload => false, :status => false }` ，這個其實就是說 chef-client 把這三個動作都自已處理了，`:restart`，這個動作就是先執行 `:stop`，再執行`:start`；`:status`就是去檢查 process 有沒有這個 service name，`:reload`，這個不知道怎麼做的=_=。 

假設

```
service "nginx" do
  supports :status => true, :restart => false, :reload => fasle
  action :start # 動作
end

```

那就是如果會執行到檢查 status 時就會去執行 `service nginx status` ，而不是去檢查 process。

<小提示> 可以讓他開機自動啟用，再來現在就開啟服務。

```
service "nginx" do
  action [:enable, :start]
end
```

### File 處理檔案

```
file "/tmp/something" do # 檔案位置
  owner 'root'           # 擁有者
  group 'root'           # 群組
  mode '0755'            # 權限
  action :create         # 動作 (如果是 :create 可省略
end
```

`:action` 的方法屬性


| Action    | Description                                                            |
| --------- | ---------------------------------------------------------------------  |
| :create   | **(預設)** 新增檔案，如果檔案存在但不符合的話，就會重新產生一個檔案覆蓋        |
| :create\_if\_missing   | 如果檔案存在就不產生檔案                                       |
| :delete   | 刪除檔案                                                                |
| :touch   | 不太懂，感覺意思是說，不管檔案是不符合，都會覆蓋檔案，所以會變動檔案的<修改時間>。|



### Template 模版

你可以建立 ERB 的檔案，利用 ruby 的寫法來製作檔案，更可以載入 attributes 的值。而 template 的檔案必需要放在 `/COOKBOOK_NAME/templates/default` 底下。Action 的方法跟 file 一樣。

```
  template “<filename>" do                        # 目標檔案位置
    source “<file name>.erb”                      # ERB 檔案來源
    mode 0755                                     # 檔案權限
    owner "root"                                  # 擁有者
    group “root"                                  # 群組
    variables({                                   # 載入 variables ，所以按照範例可以在 template，可以使用 @xxx 及 @ooo
       :xxx => node[:hello_world][:xxx][:groups],
       :ooo => node[:hello_world][:ooo][:users]
     })
    action :create                                # 動作 (如果是 :create 可省略
  end
```

### cookbook_file 從 cookbook 送檔案進去

這個檔案通常會存放在 cookbook 的 `/COOKBOOK_NAME/files/default` 裡面，跟 template 不同的是，他是原封不同的把檔案傳偷主機裡面去，Action 的方法跟 file 一樣。

```
cookbook_file "<file name>" do # 目標檔案位置
  path "<file name>"           # 來源檔案位置
  action :create               # 動作 (如果是 :create 可省略
end
```


### remote_file 從外部下載檔案

一般來說，大家在從外部下載檔案進來都是個壓縮檔，或是要執行的檔案，那 Chef 提供了一個方法可以暫存的地方，你可以用 `Chef::Config[:file_cache_path]` 來呼叫它。先暫存在那邊，之後再處理這個 remote file。

```
remote_file "#{Chef::Config[:file_cache_path]}/large-file.tar.gz" do  # 目標檔案位置
  source "http://www.example.org/large-file.tar.gz"                   # 來源檔案位置
  action :create                                                      # 動作 (如果是 :create 可省略
end
```

### execute 執行

```
  execute "<name>" do                       # 這個執行動作的名稱
    cwd "<path>"                            # 前往哪個目錄
    command "<command>"                     # 執行的 command
    not_if { ::File.exists?("<file_path>")} # 也可建立條件式
  end
```

2015.4.11 目前就先筆記這幾個，之後在寫 recipe 的時候覺得哪些重要跟常用的 resources 再慢慢補上了。
