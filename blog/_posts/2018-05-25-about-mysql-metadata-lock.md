---
title: "关于mysql meatadata lock"
description: "关于mysql meatadata lock"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - metadata lock
---

之前需要修改一个表的字段，大概是把varchar(32)改成varchar(128)，执行语句的时候发现好久都没有结果(十几分钟)，然后找运维。
她说有个事务未提交，导致meta data lock。所以这个修改语句一直在等。然后她给我看是哪个语句的事务未提交。发现不是业务逻辑里的语句，可能是我自己连只读账户的查询语句？反正先杀了再说。然后之后注意下纯查询应该不需要使用事务。杀了那个语句就好了。
