---
layout: post
title: "在AWS上安裝Octopress"
date: 2011-11-29 22:46
comments: true
categories: [Amazon Web Services (AWS) , Octopress , Ruby on Rails]
---
安裝了Octopress，感覺還蠻開心的，因為我這個人有點懶，又找不到好的設計朋友，所以只要遇到好技術又加上預設的Theme的又蠻好看的，我就會把他列入我的list裡面(感覺我好不務正業喔= =)

所以我要趕快來做個筆記好去睡覺了。

[上一篇](http://blog.igotcloud.com/hello-octopress/)有提到一些高手的文章，寫一下怎麼在`AWS`架設自己的`OctoPress`
<!--more-->

### 開啟一台EC2 & 架出WebService的環境
這邊不多說，大家可以參考我之前的文章，該文章是架出Ruby on Rails的環境，大家可以參考著看
[[Rails]Install Ruby on Rails on AWS EC2](http://blog.igotcloud.com/rails-install-ruby-on-rails-on-aws-ec2)

或是參考[[AWS]利用EC2架設Wordpress(影音)](http://blog.igotcloud.com/aws-using-ec2-set-up-wordpress-video/)

### 本機安裝Ruby的環境
這邊也可以直接參考[安裝Rails開發環境](http://ihower.tw/rails3/installation.html)
或是購買XDite的電子書[Rails 101 - 火速學會 Ruby on Rails](http://rails-101.logdown.com/)
剛剛[高見龍](http://blog.eddie.com.tw/)大大有提醒到一點，其實要編譯`OctoPress`只需要Ruby的環境而以，而我一開始就是接觸Ruby on Rails的環境，所以誤以為都是需要Ruby on Rails的，真是謝謝[高見龍](http://blog.eddie.com.tw/)大大的解說。

### 安裝Octopress 在本機端
如果你想要直接安裝在Server端的話也是可以(但是應該很少人會這樣做)，所以我們在可以到[Octopress官網](http://octopress.org/)下載或是直接用git clone到本機上
    $ mkdir project
    $ cd preject
	$ git clone git://github.com/imathis/octopress.git
### 安裝gem 初始化Octopress
	$ cd octopress
	$ bundle install
	$ rake install
### 預覽Octopress
	$ rake preview
開啟瀏覽器進入[http://127.0.0.1:4000](http://127.0.0.1:4000)應該會看到這個畫面
!["預覽Octpress"](https://lh4.googleusercontent.com/-e2GIUphxpq4/TtT7CBTTv5I/AAAAAAAABes/QOp3QMadoyo/s640/Google%252520Chrome.jpg)
### Octpress的基本設定

打開`_config.yml`你可以設定
``` ruby _config.yml
url: http://yoursite.com
#你的網址
title: My Octopress Blog
#Blog的名稱
subtitle: A blogging framework for hackers.
#BLog的副標
author: Your Name
#你的名字

```
其他詳細設定請看[官網文件](http://octopress.org/docs/configuring/)
### 建立第一篇文章
我們利用`rake`來產生一個markdown檔，它也會順便幫你建立表頭的基本設定
	$ rake new_post["post-name"]
	Creating new post: source/_posts/2011-11-29-post-name.markdown
### 開始寫第一篇文章
你可以用`gedit`或是`TextEdit`都行，不過我們都是懶人的話，建議是用[Mou](http://mouapp.com/)來編輯會比較好，因為ocropress是用markdown的語法，如果不會的話可以參考[markdown.tw](http://markdown.tw/)或是[Daring Fireball](http://daringfireball.net/projects/markdown/basics)
### 產生靜態網頁
我們用markdown寫的檔案都是放在`source\_post`利用`rake`會幫你把markdown轉成靜態網站放在`public\`
	$ rake generate
	## Generating Site with Jekyll
	unchanged sass/screen.scss
	Configuration from /project/octopress/_config.yml
	Building site: source -> public
	Successfully generated site: source -> public

### 佈署到AWS上

#### Server端
建立一個repository
	$ mkdir octopress.git
	$ cd octopress.git
	$ git init --bare
	Initialized empty Git repository in /home/ec2-user/octopress.git/
建立網站目錄及設定自動check out 最後一個版本到你的網站目錄下
	$ mkdir /var/www/octopress
	$ echo GIT_WORK_TREE=/var/www/octopressg git checkout -f >> hooks/post-receive
	$ chmod +x hooks/post-receive
#### 本機端
因為EC2是用key-pair做登入，所以我們如果不做任何修改的話，我們可以直接照以下的方法push到你的主機裡
	$ ssh-add octopuses.pem	
	#把Key-pair 加到ssh-agent
	$ git remote add web ec2-user@you-ip:/home/ec2-user/octopress.git/
	$ git add . -f
	$ git commit -m 'new octopuses'
	$ git push web
	Counting objects: 469, done.
	Delta compression using up to 4 threads.
	Compressing objects: 100% (177/177), done.
	Writing objects: 100% (253/253), 78.46 KiB, done.
	Total 253 (delta 139), reused 0 (delta 0)
	To ec2-user@ou-ip:/home/ec2-user/octopress.git/
	8ea6b76..50b9b9e  master -> master
### 設定apache
這個也可以在[[Rails]Install Ruby on Rails on AWS EC2](http://blog.igotcloud.com/rails-install-ruby-on-rails-on-aws-ec2)找的到

### 參考文章
> [How to Install Octopress on Heroku](http://blog.eddie.com.tw/2011/10/11/how-to-install-octopress-on-heroku/)

>[Why Octpress](http://blog.xdite.net/posts/2011/10/07/what-is-octopress/)

> [Octopress 搬家記 (1) -- Wordpress.com 舊文轉移](http://blog.yorkxin.org/2011/11/26/import-from-wpcom-to-octopress)

