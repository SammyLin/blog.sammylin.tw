---
layout: post
title: "[Rails]Install Ruby on Rails on AWS EC2"
date: 2011-09-28 23:34

comments: true
categories: [Amazon Web Services (AWS) , Elastic Compute Cloud(EC2) , Ruby on Rails]
---
最近在學Ruby on Rails安裝環境吃了一點苦頭，作一點記錄~~~
First Launch a instance
choose ths ami
### ami:Basic 32-bit Amazon Linux AMI 2011.09 (AMI Id: ami-dcfa4edd)

`sudo yum update`

`sudo yum upgrade`

`sudo yum install git`

`sudo yum install mysql mysql-server `

`$ sudo yum install build-essential zlib1g-dev libssl-dev libreadline5-dev`

`$ sudo yum install gcc gcc-c++ make patch openssl-devel readline-devel`

`$ sudo yum install ruby-devel ruby-irb ruby-rdoc ruby-ri`

### install RVM
`$ bash < <(curl -s https://rvm.beginrescueend.com/install/rvm)`

`$ echo "[[ -s $HOME/.rvm/scripts/rvm ]] && source $HOME/.rvm/scripts/rvm" >> ~/.bash_profile`

`$ source ~/.bash_profile`

### install REE

`$ rvm install --force ree`

### install sqlite3 and mysql

`$ sudo yum install sqlite-devel ruby-mysql mysql-devel`

### install RubyGem

`$ wget http://rubyforge.org/frs/download.php/75309/rubygems-1.8.10.tgz`

`$ tar -xvzf  rubygems-1.8.10.tgz`

`$ cd rubygems-8-10`

`$ ruby setup.rb`

### install Rails

`$ gem install rails -v=3.0.7`

`$ gem install mysql`


### install Passenger

`$ gem install passenger`

### install CUR with SSL

`$ sudo yum install curl-devel`

`$ sudo yum install httpd-devel`

### install Passenger on Apache

`$ sudo passenger-install-apache2-module`

### vim httpd.conf

`$ sudo vim /etc/httpd/conf/httpd.conf`

{% codeblock %}
LoadModule passenger_module /home/ec2-user/.rvm/gems/ree-1.8.7-2011.03/gems/passenger-3.0.9/ext/apache2/mod_passenger.so
PassengerRoot /home/ec2-user/.rvm/gems/ree-1.8.7-2011.03/gems/passenger-3.0.9
PassengerRuby /home/ec2-user/.rvm/wrappers/ree-1.8.7-2011.03/ruby

<VirtualHost *:80>
      ServerName forum.local 

    DocumentRoot /home/ec2-user/projects/forum/public 

    <Directory /home/ec2-user/projects/forum/public> 

       AllowOverride all 

       Options -MultiViews 

    </Directory>
</VirtualHost>
{% endcodeblock %}

### 建立Projects

`$ mkdir -p ~/projects/`

`$ cd ~/projects/`

`$ rails new forum`

`$ sudo chmod 705 /home/ec2-user/projects/`


Reference:

火速學會Ruby on Rails :http://rails-101.logdown.com/