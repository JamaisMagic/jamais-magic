---
title: "Logrotate with pm2"
description: "Logrotate with pm2"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - logrotate pm2
---

前两日要把node的日志导给服务端参考，主要是ip和ua，然后自己下载下来，整理了一下再给到对方。

整理主要是grep出相应日期的行，写到新文件再发给对方。

为什么要这么做？因为日志没有做切割。

为什么没有做切割？因为当初没什么时间搞，网站量也小，就先搁置。

现在看来还是需要搞好这件事，但是都是用的写业务的额外时间搞。= =

既然要搞日志切割，自然想到用Logrotate啦。

大概找了些资料了解下，然后写个配置，应该就差不多了。

<script src="https://gist.github.com/JamaisMagic/b5934a10d954e1a1600eb435783dd321.js"></script>

具体字段的作用不再做说明，说一下一些注意事项。

pm2本身提供了一个reloadLogs的命令，用于切割日志后reload日志文件。同时也可以使用系统信号kill -USR2 `cat /root/.pm2/pm2.pid`的方式通知pm2 reload日志。

另外配置文件的路径可以用空格或者换行分开，这样多个目录的日志可以用同一份配置。

有些程序配置切割日志后新的日志文件有权限限制，例如nginx，可以ls -l查看文件的权限注意一下自己登录的账户是否对该文件有相关权限。

假如你的程序没有提供reload日志的功能，或者由于某些原因不能reload日志，可能可以考虑使用copytruncate，不过这样日志会丢失一部分。

配置好之后，可以跑一下debug模式，看输出是否有问题。debug模式不会真正执行rotate，只是调试，如果想手动运行一下，可以去掉-d
```bash
logrotate -d --force /etc/logrotate.d/your_config_file
```

参考资料。

> <site><a target="_blank" href="https://linux.die.net/man/8/logrotate">logrotate(8) - Linux man page</a></site>

> <site><a target="_blank" href="http://www.gnu.org/software/libc/manual/html_node/Miscellaneous-Signals.html">24.2.7 Miscellaneous Signals</a></site>

> <site><a target="_blank" href="https://serversforhackers.com/c/managing-logs-with-logrotate">Managing Logs with Logrotate</a></site>

> <site><a target="_blank" href="https://huoding.com/2013/04/21/246">被遗忘的Logrotate</a></site>

> <site><a target="_blank" href="https://stackoverflow.com/questions/480482/linux-c-log-rotation-scheme">Linux/c++ log rotation scheme</a></site>

> <site><a target="_blank" href="https://github.com/Unitech/pm2/issues/114">Make pm2 compatible with logrotate #114</a></site>

> <site><a target="_blank" href="https://github.com/Unitech/pm2/issues/2234">logrotate script does not reload logs #2234</a></site>

> <site><a target="_blank" href="http://pm2.keymetrics.io/docs/usage/log-management/">Log management</a></site>

> <site><a target="_blank" href="https://www.digitalocean.com/community/tutorials/how-to-manage-logfiles-with-logrotate-on-ubuntu-16-04">How To Manage Logfiles with Logrotate on Ubuntu 16.04</a></site>
