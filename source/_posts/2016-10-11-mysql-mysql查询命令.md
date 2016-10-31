---
title: 2016-10-11-mysql-mysql查询命令
date: 2016-10-11 11:18:50
tags: mysql
---
Author:@南非波波

编者按:
	
	这篇文章会陆续记录在工作中使用mysql进行数据查询的一些命令，以及将一些需求和命令的实现进行结合讲解。

一、获取在(北京、广州、深圳)近三个月内有登录行为的用户信息，字段：用户名，邮箱，手机号，创建应用数量，注册时间，最后一次登录时间

	SELECT
		userName,
		loginName,
		phone,
		num,
		province,
		FROM_UNIXTIME(
			ct / 1000,
			'%Y-%m-%d %H:%i:%S'
		) AS ct,
		FROM_UNIXTIME(
			loginTime / 1000,
			'%Y-%m-%d %H:%i:%S'
		) AS loginTime
	FROM
		TB_Enterprise_User
	INNER JOIN (
		SELECT
			userId,
			COUNT(*) AS num
		FROM
			cad_app_user
		WHERE
			type = 0
		GROUP BY
			userId
		ORDER BY
			num
	) tb1 ON TB_Enterprise_User.enterUserId = tb1.userId
	WHERE
		LENGTH(phone) = 11
	AND phone IS NOT NULL
	AND loginTime >= UNIX_TIMESTAMP('2016-07-11 00:00:00') * 1000
	
	AND (
		province = '北京'
		OR province = '广州'
		OR province = '深圳'
	)
	ORDER BY
		loginTime ASC



需求分析：

	用户名，邮箱，手机号，注册时间，最后一次登录时间这几个字段都存放在一张表中(TB_Enterprise_User),只需要写好相应的条件就行。
	创建应用数需要从另一张表中(cad_app_user)进行统计查询形成新表与TB_Enterprise_User进行表关联。
	这里需要注意loginTime、ct记录的是毫秒值，而UNIX_TIMESTAMP('2016-07-11 00:00:00')转换的记录值是秒
	
二、获取过去一周新增loader用户的信息

	查询指定时间增加loader用户的信息：
		1.统计服务器每天新增文件total_day/new.txt获取appId
			awk -F "|" '{print $1}' 20161023 |sort -k 1 -n -r|uniq -c|awk -F " " '{print $2}' >> test.txt （多天就执行多条，把文件名换一下，然后执行下面一句）
			awk -F "|" '{print $1}' test.txt |sort -k 1 -n -r|uniq -c|awk -F " " '{print $2}' >> test_2.txt(获取到全部的新增loader的应用ID)
		2.根据应用ID获取对应的userId
		3.查询对应userId创建应用的数量
		4.进行表关联，查询用户表，获取用户的信息


	SELECT
		userName,
		loginName,
		phone,
		FROM_UNIXTIME(
			TB_Enterprise_User.ct / 1000,
			'%Y-%m-%d %H:%i:%S'
		) AS ct,
		province,
		num
	FROM 
		TB_Enterprise_User
	INNER JOIN (
		SELECT
			userId,COUNT(*) AS num
		FROM
			cad_app_user
		WHERE
			userId IN (
				SELECT
					userId
				FROM
					cad_app_user
				WHERE
					type = 0
				AND appId IN('A6999979578606','A6999956860949','A6999909977189')
				GROUP BY
					userId
			)
		GROUP BY
			userId
	) tb1 ON TB_Enterprise_User.enterUserId = tb1.userId
	ORDER BY
		TB_Enterprise_User.enterUserId ASC;


三、指定时间的自定义loader的使用用户
	
	SELECT
		userName,
		loginName,
		phone,
		FROM_UNIXTIME(
			TB_Enterprise_User.ct / 1000,
			'%Y-%m-%d %H:%i:%S'
		) AS ct,
		province
	FROM
		TB_Enterprise_User
	INNER JOIN loader_pack ON TB_Enterprise_User.enterUserId = loader_pack.userId
	WHERE
		loader_pack.ct >= UNIX_TIMESTAMP('2016-10-17 00:00:00') * 1000
	AND loader_pack.ct < UNIX_TIMESTAMP('2016-10-24 00:00:00') * 1000
	GROUP BY enterUserId
	ORDER BY
		TB_Enterprise_User.enterUserId ASC;
	