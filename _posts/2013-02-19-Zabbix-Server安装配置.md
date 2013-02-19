---
layout: post
title: Zabbix Server安装配置
---

Zabbix Server安装配置
========================
19 Feb 2013 - Beijing


Enviroment: CentOS-6.3-i386-minimal.iso + virtual Box

	## bridge network or hostonly network
	# vi /etc/sysconfig/network-scripts/ifcfg-eth0
	# /etc/rc.d/init.d/network restart
	# chkconfig network on

	# yum install wget make gcc mysql mysql-devel libcurl-devel net-snmp-devel openldap-devel libssh2-devel OpenIPMI-devel java-1.7.0-openjdk java-1.7.0-openjdk-devel mysql-server
	# yum install php php-mysql php-bcmath php-mbstring php-gd php-xml

	# groupadd zabbix
	# useradd -g zabbix zabbix
*************************************************************************************************

	# service mysqld start
	# mysql
		create database zabbix character set utf8;
		grant all privileges on zabbix.* to 'zabbix'@'%.%.%.%' identified by 'zabbix';
		flush privileges;
	mysql -u zabbix -p zabbix < /root/zabbix-2.0.4/database/mysql/schema.sql
	mysql -u zabbix -p zabbix < /root/zabbix-2.0.4/database/mysql/images.sql
	mysql -u zabbix -p zabbix < /root/zabbix-2.0.4/database/mysql/data.sql
*************************************************************************************************

	# wget http://sourceforge.net/projects/zabbix/files/ZABBIX%20Latest%20Stable/2.0.4/zabbix-2.0.4.tar.gz/download
	# tar -zxvf zabbix-2.0.4.tar.gz
	#./configure --enable-server --enable-agent --with-mysql --enable-ipv6 --with-net-snmp --with-libcurl --with-ldap --with-ssh2 --with-openipmi --enable-java

tips:./configure --prefix=/usr --mandir=/usr/share/man --infodir=/usr/share/info --with-bugurl=http://bugzilla.redhat.com/bugzilla --enable-bootstrap --enable-shared --enable-threads=posix --enable-checking=release --with-system-zlib --enable-__cxa_atexit --disable-libunwind-exceptions --enable-gnu-unique-object --enable-languages=c,c++,objc,obj-c++,java,fortran,ada --enable-java-awt=gtk --disable-dssi --with-java-home=/usr/lib/jvm/java-1.5.0-gcj-1.5.0.0/jre --enable-libgcj-multifile --enable-java-maintainer-mode --with-ecj-jar=/usr/share/java/eclipse-ecj.jar --disable-libjava-multilib --with-ppl --with-cloog --with-tune=generic --with-arch_32=i686 --build=x86_64-redhat-linux

*************************************************************************************************
	# vi /usr/local/etc/zabbix_server.conf
	DBUser=root --> zabbix
	DBpassword=zabbix

*************************************************************************************************
	# vi /etc/php.ini
*************************************************
	# line 440: change to Zabbix recomended
	max_execution_time = 600
	# line 449: change to Zabbix recomended
	max_input_time = 600
	# line 457: change to Zabbix recomended
	memory_limit = 256M
	# line 729: change to Zabbix recomended
	post_max_size = 32M
	# line 878: change to Zabbix recomended
	upload_max_filesize = 16M
	# line 946: uncomment and add your timezone
	date.timezone = Asia/Shanghai
************************************************

************************************************
	# mkdir -p /var/www/zabbix
	# cp /root/zabbix-2.0.4/frontends/php/* /var/www/zabbix/ -R
	# vi /etc/httpd/conf.d/zabbix.conf
		Alias /zabbix /var/www/zabbix
		<Directory "/var/www/zabbix">
		Options FollowSymLinks
		AllowOverride None
		Order allow,deny
		Allow from all # change to the range you allow to access
		</Directory>
************************************************
	mv /var/www/zabbix/conf/zabbix.conf.php.example /var/www/zabbix/conf/zabbix.conf.php
	chmod 777 /var/www/zabbix/conf/zabbix.conf.php
	vi /var/www/zabbix/conf/zabbix.conf.php
		$DB["TYPE"]                             = 'MYSQL';
		$DB["SERVER"]                   = 'localhost';
		$DB["PORT"]                             = '0';
		$DB["DATABASE"]                 = 'zabbix';
		$DB["USER"]                             = 'zabbix';
		$DB["PASSWORD"]                 = 'zabbix';
************************************************
	# cat restart_zabbix.sh
		#!/bin/bash
		service iptables stop
		ps -ef | grep zabbix_server | grep -v grep | awk '{print $2}' | xargs kill 
		ps -ef | grep zabbix_agentd | grep -v grep | awk '{print $2}' | xargs kill
		service mysqld restart
		/usr/local/sbin/zabbix_server
		/usr/local/sbin/zabbix_agentd
		/etc/rc.d/init.d/httpd restart
		************************************************
		setenforce 0

************************************************	
zabbix server is not running问题:当未关闭selinux时，zabbix frontend会不断提示zabbix server is not running。可以通过以下命令查看selinux状态。
	[root@localhost ~]# getenforce status
	Permissive
	[root@localhost ~]# 
若显示为enable，则需要setenforce 0.
具体可参考 <https://support.zabbix.com/browse/ZBX-5423>
