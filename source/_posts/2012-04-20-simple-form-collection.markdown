---
layout: post
title: "[Rails]製作下拉式選單集合"
date: 2012-04-20 10:34
comments: true
categories: [Ruby on Rails]
---

目前的狀況是這樣子的，目前是有做`CMS`所以需要有

	# Public(公開)
	# Private(私人)
	# Draft(草稿)

這三種，不過這個集合可以用類似[jeffp/enumerated_attribute](https://github.com/jeffp/enumerated_attribute)的gem配合或者是說用[twinslash/enumerize](https://github.com/twinslash/enumerize)，不過我更懶，我直接把data丟到collection裡面就好了，因為其他地方是用不大到的~

	#view/articles/_form.html.erb 
	<%= f.input :post_status, :collection => ["public", "private", "draft"]%>
	
因為我使用的是[plataformatec/simple_form](https://github.com/plataformatec/simple_form)的關係，所以他自動幫我產生一個select的下拉式選單，應該是長這樣子

![](https://lh3.googleusercontent.com/-gKzIpoD9cSE/T5DP9853W2I/AAAAAAAABwo/3fcIwnDxQq0/s800/%25E8%259E%25A2%25E5%25B9%2595%25E5%25BF%25AB%25E7%2585%25A7%25202012-04-20%2520%25E4%25B8%258A%25E5%258D%258810.50.34.png)

但是如果是需要多國語系的話

	#view/articles/_form.html.erb 
	<%= f.input :post_status, :collection => [[t(".public"),"public"], [t(".private"), "private"], [t(".draft"), "draft"]]%>
	
再來就是新增你的i18n的語系檔，我個人的習慣是另外新增一個檔案，我目前的controller叫做articles所以我建立一個i18n的中文yml檔時就會是這樣子
	# config/locales/articles.zh-TW.yml
	zh-TW:
        articles:
          form:
            public: 公開
            private: 私人
            draft: 草稿
	
然後要記得重啟server
就會成功了

![](https://lh4.googleusercontent.com/-zrcnqhAxZ4Q/T5DRrfHqOQI/AAAAAAAABw4/U64XX2hvhwE/s800/%25E8%259E%25A2%25E5%25B9%2595%25E5%25BF%25AB%25E7%2585%25A7%25202012-04-20%2520%25E4%25B8%258A%25E5%258D%258811.01.43.png)

