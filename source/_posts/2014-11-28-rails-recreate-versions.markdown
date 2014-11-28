---
layout: post
title: "[Carrierwave]重新處理縮圖"
date: 2014-11-28 13:36:20 +0800
comments: true
categories: [Rails,Carrierwave]
---

將所有資料的縮圖一並處理


    Article.find_each do |article|
      article.image.send("remove_versions!") # 移除舊的縮圖版本
      article.image.recreate_versions! # 重新建立縮圖
      article.save! # 存回資料庫
    end
