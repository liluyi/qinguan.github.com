---
layout: post
title: MySQL innodb_buffer_pool_size����
---

MySQL innodb_buffer_pool_size����
========================
04 Jan 2013 - Beijing

�ò��������� InnoDB �洢����ı����ݺ��������ݵ�����ڴ滺������С,�ʵ���������������Ĵ�С��������Ч�ļ��� InnoDB���͵ı�Ĵ��� I/O ����һ���� InnoDBΪ����ר�����ݿ�������ϣ����Կ��ǰѸò�������Ϊ�����ڴ��С��60%-80%��

�鿴mysql�Ļ����С���
	SHOW GLOBAL VARIABLES LIKE "%buffer_pool%";
����ͨ���༭/etc/my.cnf,��������������޸Ļ����С:
	innodb_buffer_pool_size = 2G

�ο����ϣ�

+ <http://blog.chinaunix.net/uid-17282739-id-3201157.html>	