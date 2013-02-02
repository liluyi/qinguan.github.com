---
layout: post
title: OpenLDAP Replication
---

OpenLDAP Replication
========================
05 Jan 2013 - Beijing

mirror��ʽ����ģʽ��֧�ֶ���̨LDAP������ͬʱд������
�༭/etc/openldap/slapd.conf,�������:
	serverID        1
	moduleload syncprov.la
	 
	overlay syncprov
	syncprov-checkpoint 100 10
	syncprov-sessionlog 100
	syncrepl        rid=001
					provider=ldaps://ldapserver.xuguojun.com:636
					bindmethod=simple
					binddn="cn=Manager,dc=xuguojun,dc=com"
					credentials=123456
					searchbase="dc=xuguojun,dc=com"
					schemachecking=on
					type=refreshAndPersist
					retry="60 +"
					starttls=yes
					tls_cacert=/etc/openldap/cacerts/cacert.pem
	mirrormode on
����ldap server��
	service slapd restart

ע������һ̨Openldap Server������ͬ������Ҫ�޸�serverID���Լ�provider.
��̨Serverʹ����ͬ����CAǩ������Բ�ͬ��hostname�޸���֤���CN.

��زο����ϣ�

+ <http://www.openldap.org/doc/admin24/replication.html>
