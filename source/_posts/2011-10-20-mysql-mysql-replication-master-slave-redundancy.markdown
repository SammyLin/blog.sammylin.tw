---
layout: post
title: ! '[Mysql]MySQL Replication(Master-Slave)備援'
categories: [Mysql]
---

![](http://blog-img.igotcloud.com/wp-content/uploads/2011/10/image1.png)

Mysql做備援的方式有很多種，這次講一個最基本的方式Master-Slave，其實網路上許多方法，大家可以去找找看，而今天的環境是使用Amazon Linux 32位元的，但是其實操作都大同小異。
看到上面那張圖，就是今天要做的，只是單純的把Database資料即時備份到另一個Slave Database裡。


<!--more-->
### Master 部份
`$ vim /etc/my.cnf`

	server-id = 1   #此id不可以重覆
	log-bin = mysql-bin  #指定產生binlog檔案的開頭檔名
`$ mysql -u root –p`

#### 建立同步用的帳號，用來給Slave database存取資料用，並給他權限Grant、Replication slave

`mysql>CREATE USER ‘同步帳號’@’%’ IDENTIFIED BY ‘密碼’;`

`mysql>GRANT REPLICATION SLAVE ON *.* TO ‘同步帳號’@’%’;`

#### 先關閉資料庫的寫入功能

`mysql >FLUSH TABLES WITH READ LOCK;`

`mysql >exit`

#### 將全部的資料庫匯出至mysql.sql

`$ mysqldump -u root -p –all-databases > mysql.sql`

#### 把sql檔給B主機也就是slave主機

`$ scp -p mysql.sql 帳號@B主機:/home/ec2-user`

####重開機之後讓他開始產生binlog

`$ service mysqld restart`

`$ mysql -u root -p`

#### 記住下面的File 跟 Position

`mysql > show master status;`


| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB |
| mysql-bin.000002 | 187146 |                      |                           |

#### 解鎖
`mysql > unlock tables;`

### Slave部份
`$ vim /etc/my.cnf`

	server-id = 2  #此id不可以重覆
	log-slave-updates   #告訴slave讀取binlog，啟動slave的重要選項之一
	log-bin = mysql-bin   #指定產生binlog檔案的開頭檔名
	binlog_format = mixed    #設定binlog的儲存格式，(maxed為預設值)
	relay-log = host_name-relay-bin    #記錄著binlog處理的過程，可以執行【FLUSH LOGS】讓mysql自動刪除較舊的檔案
	replicate-do-db = mytest    #限制slave只同步mytest資料庫的資料
	master-connect-retry = 60   #當slave無法連線至master時，間隔60秒嘗試連線(預設為60秒)

#### 把資料倒到Slave主機裡

`$ mysql -u root -p > /home/ec2-user/mysql.sql`

`$ mysql –u root –p`

#### 建立跟Master 的連結，剛剛要記的File跟Position把他填到master_log跟master_log_pos裡

`mysql > CHANGE MASTER TO MASTER_HOST='A主機', MASTER_USER='同步用帳號', MASTER_PASSWORD='密碼', master_log_file='mysql-bin.000002', master_log_pos=187146;`

#### 重新啟用mysql服務

`$ service mysqld restart`

大功告成
為了確認Slave是否有在連結跟交換資料，到Slave主機中

`$ mysql –u root –p`

`mysql > show slave status \G`

如果有出現這兩行就表示成功了！

	Slave_IO_Running: Yes
	Slave_SQL_Running: Yes
> 參考網站

> [[MySQL] 實做 MySQL Master-Master Replication 同步](http://blog.wu-boy.com/2008/12/mysql-%E5%AF%A6%E5%81%9A-mysql-master-master-replication-%E5%90%8C%E6%AD%A5/)

> [MySQL Replication(Master Slave負載平衡)](http://homeserver.com.tw/mysql/mysql-replication-master-slave%E8%B2%A0%E8%BC%89%E5%B9%B3%E8%A1%A1/)