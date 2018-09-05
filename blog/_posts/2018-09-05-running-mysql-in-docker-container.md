---
title: "Running mysql in docker container"
description: "Running mysql in docker container"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - mysql
  - docker
---

最近把一个就项目迁移到docker。涉及到数据库，就顺便把数据库也放到docker里单独运行。

有些观点认为，带状态的程序不适合用docker运行（或者委婉点的说法是docker比较适合运行那些没有状态的程序），不过我查了下docker mysql的官方镜像，发现还是挺适合用的，所以还是决定试一试。

docker镜像我用的是这个：<a target="_blank" href="https://hub.docker.com/_/mysql/">mysql</a>，也不用自己写dockerfile了，可以直接用命令或者docker compose运行，我是自己写了个docker compose的配置文件。

有几个点需要注意：
1. 这个镜像通过环境变量提供root用户密码的设置，一个用户的创建，以及一个数据库的创建。
2. 容器运行之后只需要用docker exec -it container_id mysql_command 即可运行mysql命令。
3. 假如需要在容器初始运行时创建数据库、表等，只需要把写好的mysql语句放在容器的/docker-entrypoint-initdb.d 目录里。
4. 最好把数据库数据volume到host的目录里，方便管理和防止容器销毁时数据丢失。

其他的可以参考这个镜像文档，以及我写的docker compose配置：<a target="_blank" href="https://github.com/JamaisMagic/docker_mysql">docker_mysql</a>

这种做法当然只是比较适合个人项目啦，只是用一个容器运行一个数据库实例，如果需要在production环境运行，还要考虑怎么搞集群这些东西。= =
