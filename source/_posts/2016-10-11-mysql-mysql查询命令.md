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
	