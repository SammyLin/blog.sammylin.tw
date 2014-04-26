---
layout: post
title: "[FaceBook API]搜尋好友"
date: 2012-12-04 21:12
comments: true
categories: [FaceBook API]
---

在[Graph API](https://developers.facebook.com/docs/reference/api)找了老半天
只能夠用API撈到FB的所有好友，但是沒辦法塞入q=???來找到你想要的朋友只有像`粉絲頁`、`所有使用者`..等才能用

這時候只好派出[FQL](https://developers.facebook.com/docs/reference/fql/)了

    SELECT name, uid FROM user WHERE uid IN ( SELECT uid2 FROM friend WHERE uid1=me() ) AND strpos(lower(name),'<you want search name>') ==0

<!-- more -->

馬上就可以輸入幾個關鍵字就可以抓到你要的好友名單了

就像是這樣 [DEMO](http://goo.gl/VYpbM)
(請記得取得token)

如果要做分頁的話

    SELECT uid ,name FROM user WHERE uid IN ( SELECT uid2 FROM friend WHERE uid1=me() ) AND strpos(lower(name),'<you want search name>') ==0 LIMIT 10 OFFSET 200;

就可以帶入你要的page

=======

補充一下，`FQL`是不能有COUNT()的，所以如果要取得count的話，只能把資料撈出來然後算他的數量。 不過遇到你的需要跟FB取的Field多的話，可以先用下面這行

    SELECT '' FROM user WHERE uid IN (SELECT uid2 FROM friend WHERE uid1 = USERID)

因為取得的Field少，所以response的會比較快。

## 參考資料

  > [http://blogs.x2line.com/al/archive/2007/10/07/3287.aspx](http://blogs.x2line.com/al/archive/2007/10/07/3287.aspx)
