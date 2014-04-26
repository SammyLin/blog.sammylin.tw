---
layout: post
title: "[mysql]優化"
date: 2011-12-03 09:34
comments: true
categories: [mysql]
---
因為過去都沒有接觸到mysql的環境處理，(DBA很重要的)而我最近架在`AWS
`的mysql常常會loading過重，問了一下高手才知道我沒有做MYSQL的優化，查了資料以後才知道MYSQL的優化很簡單

### 修改配置檔/etc/my.cnf

MYSQL有官方推薦的配置檔，當然是最穩最好的，官方配置檔放在`/usr/share/mysql`

	cd /usr/share/mysql/

- my-small.cnf(小於64MB的記憶體)
- my-medium.cnf (64~128MB的記憶體)
- my-large.cnf (128~512MB的記憶體)
- my-huge.cnf (1~2GB的記憶體)
- my-innodb-heavy-4G.cnf (4GB的記憶體)

可以選一個適合你的主機的配置檔並將配置檔複製到 /etc/my.cnf

	#cp  /usr/share/mysql/my-large.cnf  /etc/my.cnf

	#service mysqld restart

### 將資料表最佳化

	mysqlcheck -a -c -o -r --all-databases -uroot -p

### 重建資料表索引

	service mysqld stop

	myisamchk -s /var/lib/mysql/*/*.MYI

	service mysqld start

 

## 參考網站
> [只要十分鐘，快速優化 MySQL 資料庫](http://163.32.219.6/blog/u882061/cce-linux/2009/10/31/1237)