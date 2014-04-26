---
layout: post
title: "產生Rails專案裡所有的routes"
date: 2012-01-13 14:30
comments: true
categories: [Ruby on Rails , 筆記 , rake , routes]
---
最近有高人指點我發現頭腦越來越清晰，考試都可以考一百分了，因為剛開始摸`Ruby on Rails`的時候太多東西搞不懂了，例如：
	<%= link_to `show` , board_path(@board) %>
	<%= link_to `edit` , edit_board_path(@board) %>
	<%= link_to `back` , boards_path %>


`link_to`照字面上的意思，或是直接在瀏覽器看，很容易了解他是`超連結`的意思，第一個變數，看的出來是超連結的文字，最後呢？那是啥鬼？過去的我雖然寫過C#，跟HTML，我無法跟這一串東西連結起來，直到我一直照抄[Rails 101](http://rails-101.logdown.com/)，然後自己修改自己做一些小功能，才慢慢理解。但是用很笨的方法啦，不過原來有超簡單的方式可以解決我過去的疑惑，也是[_小蟹大師_](http://wildjcrt.pixnet.net/blog)跟我說我才知道有這個功能，Ruby on Rails真是太厲害了！

如果你對於 path不熟的話可以用這招~~只不過如果你的routes如果太多的話，就會需要比較久的時間產生出所有URL Helper、URL 網址和對應的Controller Action。

	$ rake routes
	             board_posts GET    /boards/:board_id/posts(.:format)          {:controller=>"posts", :action=>"index"}
                         POST   /boards/:board_id/posts(.:format)          {:controller=>"posts", :action=>"create"}
          new_board_post GET    /boards/:board_id/posts/new(.:format)      {:controller=>"posts", :action=>"new"}
         edit_board_post GET    /boards/:board_id/posts/:id/edit(.:format) {:controller=>"posts", :action=>"edit"}
              board_post GET    /boards/:board_id/posts/:id(.:format)      {:controller=>"posts", :action=>"show"}
                         PUT    /boards/:board_id/posts/:id(.:format)      {:controller=>"posts", :action=>"update"}
                         DELETE /boards/:board_id/posts/:id(.:format)      {:controller=>"posts", :action=>"destroy"}
                  boards GET    /boards(.:format)                          {:controller=>"boards", :action=>"index"}
                         POST   /boards(.:format)                          {:controller=>"boards", :action=>"create"}
               new_board GET    /boards/new(.:format)                      {:controller=>"boards", :action=>"new"}
              edit_board GET    /boards/:id/edit(.:format)                 {:controller=>"boards", :action=>"edit"}
                   board GET    /boards/:id(.:format)                      {:controller=>"boards", :action=>"show"}
                         PUT    /boards/:id(.:format)                      {:controller=>"boards", :action=>"update"}
                         DELETE /boards/:id(.:format)                      {:controller=>"boards", :action=>"destroy"}
                    root        /                                          {:controller=>"boards", :action=>"index"}


