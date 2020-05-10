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
  4. 链路层 Link layer

### HTTPS

HTTPS即**Hypertext Transfer Protocol Secure**，超文本安全传输协议。它不是单独分离的协议，而指的是http over an encrypted tls/ssl connection。

HTTP请求发生在TCP/IP模型的最高层，Application Layer。HTTPS也一样，但是属于该层中相比HTTP更低层次。该协议将HTTP消息加密并按照顺序传输，当加密数据到来时进行解密。

使用 [Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) （TLS）（Note: TLS前任为SSL）进行加密解密。SSL连接确保数据被想要的人发送或接收，阻止中间人攻击。这确保了即使加密数据在传输过程中被拦截，在没有private key的情况下加密数据的真实内容不会被修改。

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

  如果访问网站使用HTTP协议，则直接基于TCP协议发送HTTP请求：请求报文 + 请求内容

  如果使用HTTPS协议：建立TLS连接。建立TLS连接首先进行SSL handshake(communication between client and server)，顺序如下：

  1. The client sends a message to the server to identify itself. The message contains information about which encryption and compression algorithms the browser supports, and other information that the server needs to communicate with the client using SSL.
  2. The server sends its digital certificate and also other information that the client needs to communicate with the server using SSL.
  3. The client (browser) verifies the digital certificate authority by checking it against a list of known certificate authorities.
  4. To verify the private key, the client will generate a *pre-master secret* using the servers **public key** (provided by the certificate authority), the server then uses its **private key** to decrypt the *pre-master secret*.
  5. At the end of the SSL handshake, a master secret key is generated which is used to encrypt and decrypt information sent across the network.

- 

### Reference

- https://medium.com/@oluwayemisi/https-how-ssl-tls-works-in-the-browser-and-why-you-need-it-89c71dfeaa6f
- https://en.wikipedia.org/wiki/Internet_protocol_suite