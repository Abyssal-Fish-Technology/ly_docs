---
title: 管理服务编译安装
linkTitle: 管理引擎
description: 流影管理服务模块ly_server编译安装说明
date: 2022-12-22
publishdate: 2022-12-28
categories: [installation,compile]
menu:
  docs:
    parent: installation
    weight: 30
toc: true
weight: 30
isCJKLanguage: true
---


​ly_server是流影的管理引擎，用于聚合分析引擎产出的威胁事件、数据节点管理、用户管理、配置管理、数据查询等。

## 项目地址

* [Github](https://github.com/Abyssal-Fish-Technology/ly_server)
* [Gitee](https://gitee.com/abyssalfish-os/ly_server)

## 目录结构

```
ly_server
├───dependence		-- 依赖环境
├───src
	├───common		-- 数据结构与通用工具函数
	├───lib			-- 工具库函数
	├───server		-- 引擎主体代码
		├......		-- 各API实现
```



## 环境依赖

```text
# 操作系统
CentOS-7-x86_64-Minimal-2009

# 编译环境
gcc、gcc-c++、cmake
bison、flex
json-c-devel
boost-devel
libcurl-devel
mariadb-server
mariadb-devel
httpd、ntp
cgicc-3.2.16
cppdb-0.3.1
protobuf-3.8.0
```


## 安装依赖

```sh
# 1. 安装依赖组件
	yum install gcc gcc-c++ cmake -y
	yum install bison flex json-c-devel -y
	yum install ntp -y
	yum install httpd -y
	yum install boost-devel -y
	yum install libcurl-devel -y
	yum install mariadb-server mariadb-devel -y

# 2. 编译安装cgicc
	tar -zxvf cgicc-3.2.16.tar.gz -C ./
	cd ./cgicc-3.2.16
	./config
	make && make install

# 3. 编译安装cppdb
	tar -jxvf cppdb-0.3.1.tar.bz2 -C ./
	cd ./cppdb-0.3.1
	cmake -DCMAKE_INSTALL_PREFIX=/usr -DLIBDIR=lib64 -DMYSQL_LIB=/usr/lib64/mysql/libmysqlclient.so -DMYSQL_PATH=/usr/include/mysql
	make && make install

# 4. 编译安装protobuf-3.8.0
	tar -xzvf protobuf-3.8.0.tar.gz
	./configure
	make && make install
	ln -sf /usr/local/lib/libprotobuf.so.19.0.0 /usr/lib64/libprotobuf.so.19

```



## 编译部署

```sh
# 5. 创建目录结构
   mkdir -p /home/Server
   ln -s   /home/Server/  /Server


# 6. 编译源代码
	cd src

	# 编译common
	cd common
	make && make install

	# 编译lib
	cd lib
	make && make install

	# 编译server
	cd server
	make && make install
```



## 运行配置

```sh
# 7. 配置语言和时区
   export LANG=en_US.UTF-8
   ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime>/dev/null 2>&1
   ntpdate cn.pool.ntp.org

# 8. 关闭selinux、开启防火墙端口
   #编辑config⽂件
   vi /etc/selinux/config
   #找到配置项
   SELINUX=enforcing
   #修改配置项为
   SELINUX=disabled
   #执⾏命令，即时关闭selinux
   setenforce 0

	 systemctl restart firewalld
	 firewall-cmd --zone=public --add-port=18080/tcp --permanent
	 firewall-cmd --reload

# 9. 配置httpd
   # 编辑/etc/httpd/conf.d/server.conf
   Listen 18080
   <VirtualHost :18080>
      DocumentRoot "/Server/www"
      AddDefaultCharset utf-8
      <Directory "/Server/www">
          Options FollowSymLinks Includes
          XBitHack on
          AllowOverride None
          Order allow,deny
          Allow from all
          Require all granted
      </Directory>
      Alias /d/ "/Server/www/d/"
      <Directory "/Server/www/d/">
          Options ExecCGI FollowSymLinks
          SetHandler cgi-script
          AllowOverride None
          Order allow,deny
          Allow from all
          Require all granted
          RewriteEngine On
          RewriteCond %{REQUEST_FILENAME} !auth$
          RewriteRule ^(.)$ auth?auth_target=$1 [QSA,PT,L]
      </Directory>
   </VirtualHost>

   #重启httpd
   systemctl restart httpd

# 10. 配置mariadb
    mkdir -p /Server/etc

    #编辑/Server/etc/gl.server.cnf⽂件
    vi	/Server/etc/gl.server.cnf

    #添加如下内容
    [gl.server]
    default-character-set=utf8
    user=root
    database=server
    passwd=


    cp /Server/etc/gl.server.cnf /etc/my.cnf.d/

		#启动数据库
		systemctl	start	mariadb
		#初始化数据库，根据提示操作即可（⽆登录密码，拒绝⾮本地登录）
		mysql_secure_installation
		#登陆本地数据库
		mysql  -uroot
		#新建数据库server
		create	database	server;
		#选择server数据库
		use	server
		#导⼊数据
		source	/root/db.server.v1.0.0.clean.sql
		#导⼊成功后，退出
		exit
		#重启mariadb
		systemctl  restart  mariadb

```



## 定时任务

```	sh
# 11. 创建定时任务
    编辑/var/spool/cron/root，写入内容：
    */5 * * * * /Server/bin/config_pusher
    */5 * * * * /Server/bin/gen_event
```

## 开源许可

* GNU GENERAL PUBLIC LICENSE  Version 3

## 联系方式

如果在开发、部署、使用的过程中遇到任何问题，或者技术讨论、产品咨询、商务合作等，都欢迎前来联系我们！

联系邮箱：[opensource@abyssalfish.com.cn](mailto:opensource@abyssalfish.com.cn)

