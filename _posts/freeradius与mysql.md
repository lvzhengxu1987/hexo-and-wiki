---
title: freeradius、mysql与openvpn
date: 2016-06-18 15:38:21
tags: [freeradius,mysql,openvpn]
categories: tools
---

搭建freeradius服务器,并搭建mysql数据库配合使用

1. 安装freeradius (如果是公共机器，先看看1812和1813端口是否已经被别人的radius占用着，此时需要自己找配置的地方改改) 编译freeradius代码，在编译代码的时候，为了不影响系统中其他程序，我把freeradius的代码整个编译到一个自定义的文件夹中。路径是 /xx/freeradiusd/freeradius-server-3.0.11,此次安装编译都是debug版本，没有做rpm安装包之类的。
<!--more-->
2. 在编译前先安装mysql软件。 ``yum install -y mysql-server.x86_64  mysql-devel.x86_64 libtalloc.x86_64 libtalloc-devel.x86_64`` 然后设置freeradius的configure选项。``./configure --prefix=/home/lvzhx/freeradiusd/usr/ --disable-openssl-version-check --enable-developer`` 然后  ``make && make install`` 之后修改freeradius的配置，并创建freeradius在mysql中的数据库。freeradius的配置都在这个目录中 /xx/freeradiusd/usr/etc/raddb，要改动的方面包括client的配置，数据库的配置，还有用户验证时的选项配置。
 改动一：client的配置，再改动前，先说一下freeradius client的概念。在radius中，对他而言的client一般指的是vpn的服务器，路由器等等，这些设备对我们而言，我们一般是拿着vpn的客户端去连接vpn的服务器；或者是拿着个人电脑，去连接路由器，等待路由器给我的个人电脑分配ip信息，等等。所以，这个角度上看，我们的vpn客户端是一个client，然后vpn服务器是server，同时，VPN服务器在fradius看来也是一个client，radius本身是一个server。 修改client的配置路径是：/xx/freeradiusd/usr/etc/raddb/clients.conf,在这个文件中要修改的内容如下： <pre>client vpn01 {
     ipaddr          = 192.168.40.251 
     secret          = testing123     
}</pre> 在这里面vpn01是一个名称，表示这个client叫什么。ipaddr表示vpn服务器的ip，如果在外网，ip必须是真实的ip。secret是一段和client事先约定好的密钥。 把client改完后，就要修改radius中的sql配置，进入/xx/freeradiusd/usr/etc/raddb/mods-enabled，执行ln -s ../mods-available/sql sql 然后 vim  sql; 修改一下几项。<pre>
  把 driver = "rlm_sql_null"  改成 driver = "rlm_sql_mysql"
  把 dialect = "sqlite" 改成 dialect = "mysql"
  把 server = "localhost" 及以下4行的注释都去掉
</pre> 把sql配置改完后，就开始改动/xx/freeradiusd/usr/etc/raddb/sites-enabled里面的default,将authorize里面的files注释掉，将sql打开,将accounting里面的sql打开 把post-auth里面的sql打开，还有post-auth里面的Post-Auth-Type REJECT里面的sql也打开

3. 接下来就要修改mysql配置信息，首先修改mysql数据库的编码选项，让数据库的编码配置都是utf-8 <pre>
 [mysqld]
 datadir=/var/lib/mysql
 socket=/var/lib/mysql/mysql.sock
 user=mysql
 symbolic-links=0
 character-set-server=utf8
 event_scheduler=1
 [mysql]
 default-character-set=utf8
 [mysqld_safe]
 log-error=/var/log/mysqld.log
 pid-file=/var/run/mysqld/mysqld.pid </pre> <pre>
 在mysql创建freeradius相关数据库.
 mysql -uroot -proot用户的密码
 mysql> create database radius;
 mysql> quit
</pre> 接下来就是创建radius相关数据库，在/xx/freeradiusd/usr/etc/raddb/mods-config/sql/main/mysql中，有 setup.sql schema.sql，其中setup.sql是创建用户和增加授权的，schema.sql是在数据库中创建表的。执行顺序是1.setup.sql，2。schema.sql 使用``mysql -uroot -p密码 radius < setup.sql``创建用户并授权, 创建用户完毕后，就可以创建数据库表了。执行代码 schema.sql ``mysql -uroot -p密码 radius < schema.sql``

4. 上述步骤完成后，就可以试着启动freeradius。debug启动方式是 `` /xx/freeradiusd/usr/sbin/radiusd -XX `` 当看到一些内容输出时 <pre>
Thu Jun 16 17:10:33 2016 : Debug: Listening on auth address 127.0.0.1 port 18120 bound to server inner-tunnel
Thu Jun 16 17:10:33 2016 : Debug: Listening on auth address * port 1812 bound to server default
Thu Jun 16 17:10:33 2016 : Debug: Listening on acct address * port 1813 bound to server default
Thu Jun 16 17:10:33 2016 : Debug: Listening on auth address :: port 1812 bound to server default
Thu Jun 16 17:10:33 2016 : Debug: Listening on acct address :: port 1813 bound to server default
Thu Jun 16 17:10:33 2016 : Debug: Opened new proxy socket 'proxy address * port 53811'
Thu Jun 16 17:10:33 2016 : Debug: Listening on proxy address * port 53811
Thu Jun 16 17:10:33 2016 : Debug: Opened new proxy socket 'proxy address :: port 37099'
Thu Jun 16 17:10:33 2016 : Debug: Listening on proxy address :: port 37099
Thu Jun 16 17:10:33 2016 : Info: Ready to process requests
</pre> 基本可以确定freeradius启动成功

5. 安装openvpn，在另外一台服务器机器上。路径是/xx/lx-openvpn-radius-server/openvpn-2.3.10里面，`` ./configure && make `` 接下来编译 openvpn radius plugin，此处需要先 ``sudo apt-get install libgcrypt20 libgcrypt20-dev`` plugin的目录是/xx/lx-openvpn-radius-server/plugin/radiusplugin_v2.1a_beta1,直接``make``即可,然后将编译出来的so文件放到/xx/lx-openvpn-radius-server/etc中.  接下来修改/xx/lx-openvpn-radius-server/etc中的server.conf(openvpn配置文件)和radius.conf(radius plugin配置文件) server.conf主要修改``plugin /xx/lx-openvpn-radius-server/etc/radiusplugin.so /xx/lx-openvpn-radius-server/etc/radius.conf``, radius.conf主要修改<pre>
 NAS-IP-Address=VPN server IP
 OpenVPNConfig=/xx/lx-openvpn-radius-server/etc/server.conf
 server中的：
 name=radius server ip
 sharedsecret=testing123(此处注意，密钥一定要一致)
</pre> openvpn debug启动命令是 ``/xx/lx-openvpn-radius-server/openvpn-2.3.10/src/openvpn/openvpn --cd /xx/lx-openvpn-radius-server/etc --config /xx/lx-openvpn-radius-server/etc/server.conf --daemon``

在测试的时候，切记openvpn客户端的证书要和服务器的证书配套。

参考地址
1.http://bbs.csdn.net/topics/350194228
