---
title: java 网络编程
date: 2024-12-10T10:39:07+08:00
draft: false
comments: true
categories: "学习笔记"
tags: ["java"]
---

## 网络编程概述
网络编程主要解决计算机与计算机之间的数据传输问题

- 网络编程三要素：ip地址、端口号、协议

## IP地址与端口、子网

### IP地址
Internet上每台主机（Host）都有一个唯一的ip地址。  
常用的ipv4地址由32位二进制数组成，通常用点分十进制表示，如192.168.1.1

#### IP地址分类
- A类地址：一个网络号+三个主机号 政府单位使用
- B类地址：两个网络号+两个主机号 事业单位使用
- C类地址：三个网络号+一个主机号 私人使用

>特殊的ip地址：127.0.0.1 本机回环地址

### InetAddress
java.net包下的一个类，表示ip地址，其功能包括：
#### InetAddress 类常用方法简介

- `getLocalHost()`
  - 返回表示本地主机的 `InetAddress` 对象。
- `getByName("IP 或主机名")`
  - 根据提供的 IP 地址字符串或主机名返回 `InetAddress` 对象。
- `getHostAddress()`
  - 返回 IP 地址的字符串形式。
- `getHostName()`
  - 返回该 IP 地址对应的主机名。

### 端口

（此处的端口指逻辑端口）
一个ip地址有2^16个端口，好比一扇房子可以有很多个门。其中
- 0~1023为公认端口，紧密绑定一些服务，例如http默认使用80端口
- 1024~49151为注册端口，松散绑定一些服务
- 49152~65535为动态/私有端口

### 子网

网络中的每一个计算机都有一个 IP 地址， IP 地址由四个数字表示，包括网络号和主机号两部分；例如 172.16.1.101 ，其中 172.16.1 是网络号，也是常说的网段，101 是主机号；网络号标识的就是上的一个**子网**；只有在一个子网中计算机之间才能“直接”互通

## 网络协议

### TCP/IP协议栈
TCP/IP协议栈是互联网通信的基础，它由一系列协议组成，下面是其中一部分

#### 网络层

- IP (Internet Protocol)：互联网协议，负责将数据包从源地址传输到目的地址。
- ICMP (Internet Control Message Protocol)：互联网控制消息协议，用于发送错误报告和其他信息，比如“目的地不可达”消息。
- IGMP (Internet Group Management Protocol)：互联网组管理协议，用于管理和维护多播组成员资格。

#### 传输层

- TCP (Transmission Control Protocol)：传输控制协议，提供可靠的数据传输服务，确保数据包按顺序到达，并处理数据包丢失和错误。
- UDP (User Datagram Protocol)：用户数据报协议，提供不可靠的数据传输服务，不保证数据包按顺序到达，也不处理数据包丢失和错误。

#### 应用层

- HTTP (HyperText Transfer Protocol)：超文本传输协议，用于在互联网上传输网页数据。
- FTP (File Transfer Protocol)：文件传输协议，用于在互联网上传输文件。。
- DNS (Domain Name System)：域名系统，用于将域名解析为 IP 地址。
- ARP (Address Resolution Protocol)：地址解析协议，用于将 IP 地址解析为物理地址。

### TCP协议

- TCP协议是面向连接的协议，它提供可靠的、有序的、无差错的传输服务。  
- TCP协议通过三次握手建立连接，四次挥手断开连接。
- 通信前必须建立连接，效率稍低
- 使用字节流发送数据
