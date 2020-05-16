---
layout: post
title:  "记录Archlinux安装MariaDB记录"
date:   2020-05-16 10:18:50 +0800
categories: database
---

### MariaDB介绍
MariaDB是MySQL关系型数据库的一个分支（fork），由社区开发并提供商业化支持。为了保持免费和在GNU许可下开源而创建。

### 操作环境
- ArchLinux 2020-5-16滚动更新至最新
- KDE桌面环境

### 安装MariaDB
```bash
sudo pacman -S mariadb # 结果：安装完成
```
### 配置MariaDB

```bash
sudo mysql_install_db --datadir=<database-dir> # WARNING: 修改<database-dir所有者和用户组>，参见启动mariadb.service报错。
```
添加数据库路径到MariaDB配置文件（默认使用/etc/my.cnf）
```text
[mysqld]
...  
datadir=/path/to/directory
```

### 启动mariadb.service
```bash
sudo systemctl start mariadb # 结果：报错
```

报错：
```bash
mysqld[1169]: 2020-05-16 11:20:13 0 [Warning] Could not increase number of max_open_files to more than 16364 (request: 32186)

mysqld[1169]: 2020-05-16 11:20:13 0 [Warning] Can't create test file /mnt/Lin/mysql/ArchLinux.lower-test
```
解决：
编辑/usr/lib/systemd/system/mariadb.service，修改LimitNOFILE为infinity。然后systemctl daemon-reload重新载入服务。

报错:
```bash
mysqld[2789]: 2020-05-16 11:27:26 0 [Warning] Can't create test file /mnt/Lin/mysql/ArchLinux.lower-test
```
解决：
mariadb服务以mysql作为用户启动，第二步使用root身份生成数据库，所以无法没有权限在数据库文件夹下创建文件。
```bash
sudo chown mysql:mysql -R /mnt/Lin/mysql # 修改路径下的所有文件的用户和用户组为mysql
```

### 登录mariadb

第二步配置mariadb时会创建两个账户: 普通账户 & root，执行
在两个用户之一的shell环境下执行
```bash
mariadb
```
进入mariadb命令行管理界面。

