---
layout: post
title:  "HTTPS、SSL、Certificate Authority 概念、认知、创建自己的CertificateAuthority"
date:   2020-05-09 10:18:50 +0800
categories: jekyll update
---

### 基础知识

- TCP/IP传输模型

  所有TCP/IP系列网络协议都被归类为4个“抽象层”。

  1. 应用层 Application Layer
  2. 传输层 Transport Layer
  3. 网络互连层 Internet Layer
  4. 网络访问层 Network Access Layer

- 

## 使用WebBrowser访问HTTPS站点的发生了什么？

- 如果URL包含域名则进行DNS解析得到主机的IP地址，如果包含IP则直接为主机IP地址。
  DNS解析来源：

  1. 浏览器DNS缓存
  2. 操作系统DNS缓存
  3. 操作系统hosts文件
  4. 操作系统DNS服务器查询

- TCP三次握手与目标主机建立连接（发生在传输层）

  传输层解决诸如端到端可靠性和保证数据按照正确顺序到达这样的问题。常用的传输层协议有：

  - TCP： 可靠的、面向“链接”的传输机制

    TCP协议提供一种可靠的字节流保证数据完整、无损并且按顺序到达。

  - UDP：无链接的数据报协议

    该协议不检查数据包是否到达目的地且不保证传输顺序。

  HTTP/HTTPS协议属于应用层，建立在TCP链接之上。

- 发送HTTP/HTTPS请求

  

- 
