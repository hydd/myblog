---
title: Linux下对端口流量进行统计
date: 2019-03-16 18:01:55
tags:
	- Linux
categories:
	- 资源分享
---



利用Linux中自带的iptable添加简单的规则让其起到端口流量统计的作用。但是需要注意的是在服务器重启、iptable服务重启的时候统计数据会被重置清零。

## **添加需要统计的端口**

### 输入监控

下面示例是监控目标端口是443的输入流量

```shell
iptables -A INPUT -p tcp —dport 443
```

### 输出监控

下面示例是监控来源端口是443的输出流量

```shell
iptables -A OUTPUT -p tcp —sport 443
```

## **查看统计数据**

```
iptables -Lvn
```

示例结果：
443端口接收的流量为34m，发送的流量是24m

![](./1.png)

## **重置统计数据**

**注意：这里是重置所有端口的统计数据**

### 重置所有输入端口

```shell
iptable -Z INPUT
```

### 重置所有输出端口

```shell
iptable -Z OUTPUT
```

## **移除统计端口**

### 移除输入端口

```shell
iptables -D INPUT -p tcp —dport 443
```

### 移除输出端口

```
iptables -D OUTPUT -p tcp —sport 443
```