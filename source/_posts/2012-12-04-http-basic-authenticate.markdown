---
layout: post
title: "[Rails] 進入網站前驗證"
date: 2012-12-04 20:41
comments: true
categories: [Ruby on Rails]
---

有時候專案尚未公開時，還是需要放到網路上讓其他人來瀏覽，這時候為了怕非相關人員看到這個網站的話，就需要有個關卡，通常都會在nginx或apache來設定，但是這往往還要進到server去處理。
如果在rails上你不想這麼麻煩的話，可以在`ApplicationController`上直接加上這一行

    http_basic_authenticate_with name: "admin", password: "pw"

就像下面這樣

``` ruby
class ApplicationController < ActionController::Base  protect_from_forgery
  http_basic_authenticate_with name: "admin", password: "pw"
  protect_from_forgery
end

```

當你在進入網站的時候就會需要你輸入帳密才能進入了

![](https://lh6.googleusercontent.com/-RYBzUO6WDHE/UL3yLnBSaEI/AAAAAAAACBQ/hB0LWke0YaA/s800/b2c64cda0232f18a97b8a215bc138e7b.png)


