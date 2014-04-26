---
layout: post
title: "[Jquery]設定下拉選單的初始值"
date: 2011-11-30 11:13
comments: true
categories: [Jquery]
description: "記錄如何在Jquery上設定下拉選單的初始值"
---

``` html
<select id ="selectbox">
<option value="google">google</option>
<option value="yahoo">yahoo</option>
<option value="bing">bing</option>
</seltion>
```

``` js
$('#selectbox option[value=google]').attr('selected','selected');
```