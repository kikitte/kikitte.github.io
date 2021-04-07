---
layout: post
title:  "安装并使用PostGIS的Docker镜像"
date:  2021-04-07 22:44:52 +0800
categories: geographic
---

1. 我为什么要使用PostGIS Docker镜像

   根据[官方博客](https://blog.cleverelephant.ca/2020/12/waiting-postgis-31-1.html)，PostGIS3.1的版本带来了显著的性能提升，因此版本升级是值得的。但是在我所使用的ArchLinxu操作系统中的PostGIS版本仍然是3.0，所以无法及时更新到新版本的PostGIS一探究竟。故而转为使用Docker镜像来使用最新版本的软件包。

2. 安装过程

   - 选择镜像

     针对不同版本的PostgreSQL和PostGIS排列组合，官方提供了多种选择，这里当然是选择最新的版本了。PostgreSQL为13，PostGIS为3.1。

   - 拉取镜像

     ```bash
     # Note: 使用root帐户执行命令
     docker pull postgis/postgis:13-3.1
     ```

   - 运行容器

     ```bash
     docker run -d --name postgis -e POSTGRES_PASSWORD=your_password -p 5432:5432 -v /home/xxx/pg-data:/var/lib/postgresql/data  postgis/postgis:13-3.1
     ```

     这里创建了一个名为postgis的容器，将主机端口5432映射到容器端口5432，设定postgresql数据目录挂载到/home/xxx/pg-data以使得数据持久化，注意，环境变量`POSTGRES_PASSWORD`是必须的，且不能为空，该变量指定了PostgreSQL超级用户(无认为postgres)的密码.

   - 连接PostgreSQL

     可以通过Unit socket 或 TCP 连接PostgreSQL数据库，由于上述运行容器过程中已经进行了端口映射，所以可以直接使用psql工具进行连接.

     ```bash
     psql -h localhost -p 5432 -U postgres
     ```

     Note: 如果其他容器需要使用该数据库则需要创建网络作为客户端与数据库之间连接的渠道。