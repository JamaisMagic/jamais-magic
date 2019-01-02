---
title: "Mysql export csv"
description: "Mysql export csv"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - Mysql
---

之前做了一个短链接服务，后来想迁移到另一台机器，查理一下数据库，发现也有别人在用，上面有一些数据，然后需要把数据备份下来，用mysqldump导出了一份sql文件。

再然后想直观地看看里面的数据（不多，也就几百条），就想把数据导出到csv文件里。

网上搜了一下，发现mysql可以直接导出到csv，语句大概如下：

```sql
select * from tablename into outfile '/var/lib/mysql-files/urls.csv' fields terminated by ',' enclosed by '"' lines terminated by '\n';
```

这个语句需要注意文件的路径问题，一开始我想直接把文件导出到用户目录，但是执行的时候提示

```bash
ERROR 1290 (HY000): The MySQL server is running with the --secure-file-priv option so it cannot execute this statement
```

这个--secure-file-priv的意思大概是文件只能放到有权限的目录，继续google了一下，发现可以通过执行语句

```sql
show variables like "secure_file_priv";
```

查询这个目录，然后上面的路径改为这个即可，一般应该是 /var/lib/mysql-files/

参考网页：

* <site><a target="_blank" href="https://stackoverflow.com/questions/356578/how-to-output-mysql-query-results-in-csv-format">How to output MySQL query results in CSV format?</a></site>

* <site><a target="_blank" href="https://stackoverflow.com/questions/32737478/how-should-i-tackle-secure-file-priv-in-mysql">How should I tackle --secure-file-priv in MySQL?</a></site>
