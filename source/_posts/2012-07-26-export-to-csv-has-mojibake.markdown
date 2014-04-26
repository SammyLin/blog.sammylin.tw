---
layout: post
title: "[Ruby]匯出csv檔時在excel開啟時亂碼"
date: 2012-07-26 09:40
comments: true
categories: [Ruby on Rails]
---

做個筆記吧..前幾次都是開箱文..哈哈，感覺有點偷懶

以前在`ASP.NET`的時候要把資料倒進Excel或是CSV，是不怎麼痛的，畢竟都是同一家人嘛。但是我今天要在`Ruby`中倒資料進去CSV的時候，就會有編碼上的問題。

我們在`Ruby`中如果要有中文的話，其實我們都會在.rb最上面加一行

    # encoding : utf-8

如果要匯出CSV的話`Ruby`在有一個[Class](http://ruby-doc.org/stdlib-1.9.2/libdoc/csv/rdoc/CSV.html)可以用

{% codeblock lang:ruby %}
    # encoding : utf-8
    require 'CSV'

    def shom_method

      CSV.generate do |csv|
        csv << ['編號', '姓名']
        members.each do |member|
	  csv << [num, name]
        end
      end

    end
{% endcodeblock %}

用PC的筆記本打開看....沒錯是ok的！
然後用Excel一開...哇勒~是亂碼..

查了一下是因為我一開始設定的編碼是`utf-8`，所以我匯出檔案的編碼會是`utf-8`，因為Excel開的時候就會出現亂碼...

不過有一個笨方法...叫User把csv檔用筆記本打開之後用`ANSI`存檔後用Excel開就ok啦.....

但是我沒這個膽...

## 解決方式

我也沒有找到一個很好的處理方式...

目前比較簡單的方式其實就是在檔頭加上`\uFEFF`字元

{% codeblock lang:ruby %}
    # encoding : utf-8
    require 'CSV'

    def some_method
      "\uFEFF#{get_csv}"
    end

    def get_csv

      CSV.generate do |csv|
        csv << ['編號', '姓名']
        members.each do |member|
	  csv << [num, name]
        end
      end

    end
{% endcodeblock %}

但是這個方法在Excel 2007，還是會亂碼，不過在Excel 2003、2010都是可以。

如果有人可以提供更好的方法就留個給給我吧...
