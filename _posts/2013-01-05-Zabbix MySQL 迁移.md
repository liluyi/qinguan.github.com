---
layout: post
title: Zabbix MySQL Ǩ��
---

Zabbix MySQL Ǩ��
========================
05 Jan 2013 - Beijing 
 
�����������ݲ��ر����ݿ⣺
	mysqldump -u root -p password > /tmp/zabbix_db_dump
	service mysql stop
���»����ϴ������ݿⲢ���룺
	Create database zabbix;
	mysql -u root -p password < /tmp/zabbix_db_dump
	mysql -u root -p password
����zabbix�û�:
	create user ��zabbix��@'IP Address�� identified by ��Password��;
	use zabbix;
	grant all privileges on zabbix.* to ��zabbix��@'IP Address��;
	flush privileges;
	exit;
ע: IP Address ��ѡ��ʹ��'%.%.%.%'ģʽ��
	mysql -u zabbix -p -h MySQLServerIPAddress
	
����zabbix����
First, stop Zabbix Agent and Server.
	sudo /etc/init.d/zabbix-agent stop
	sudo /etc/init.d/zabbix-server stop
Edit the following file: /etc/zabbix/zabbix_server.conf.Search for DBHost. 
Update the line to:
	DBHost=IP Address
	DBPassword=Password
����zabbix����
	sudo /etc/init.d/zabbix-server start
	sudo /etc/init.d/zabbix-agent start
����zabbixǰ�ˣ�
Edit the following file: /zabbix/conf/zabbix.conf.php. 
Search for $DB["SERVER"] and change the ip address to IP Address.
	$DB['SERVER'] = 'IP Address';
	$DB['PASSWORD'] = 'password';
 
 
