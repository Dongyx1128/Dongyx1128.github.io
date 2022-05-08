---
title: 阿里云盾彻底卸载，屏蔽IP不在恢复
date: 2020-10-06 08:05:20
categories: 
- 服务器
- 阿里云
- 卸载阿里云盾
tags: 
- 服务器
- 阿里云
- 阿里云盾
---
## 关于阿里云盾

&emsp;&emsp;用过阿里云ecs服务器或者产品的人都知道，阿里云是自带安全扫描还有云盾防御的，虽然表面上是说给你安全防御，其实就是为了获取你的数据。

&emsp;&emsp;另外还可以记录一些攻击记录，让你付费解决，不过都没什么卵用，该被攻击的还是会被攻击，该花钱解决问题的一个子都少不了。

&emsp;&emsp;如果自己服务器天天要被阿里云扫描，那么有什么违规关键词都会给你封了，特别是小站，没人气就没攻击，不过阿里云会天天攻击你，告诉你网站漏洞。

&emsp;&emsp;最后呢，要么你自己解决，要么你就花钱解决，对于强迫症的人来说，阿里云盾给你带来的是无时无刻的折腾。

<!-- more -->

## 卸载阿里云盾

&emsp;&emsp;远程连接到阿里云云服务器或者轻量应用服务器后，执行以下代码卸载阿里云盾：

```
wget http://update.aegis.aliyun.com/download/uninstall.sh
chmod +x uninstall.sh
./uninstall.sh
wget http://update.aegis.aliyun.com/download/quartz_uninstall.sh
chmod +x quartz_uninstall.sh
./quartz_uninstall.sh
```

## 删除阿里云盾文件残留

&emsp;&emsp;卸载阿里云盾后，执行如下代码删除阿里云盾文件残留：

```
pkill aliyun-service
rm -fr /etc/init.d/agentwatch /usr/sbin/aliyun-service
rm -rf /usr/local/aegis*
```

## 屏蔽阿里云盾IP

&emsp;&emsp;最后就是屏蔽阿里云盾的IP：

```
iptables -I INPUT -s 140.205.201.0/28 -j DROP
iptables -I INPUT -s 140.205.201.16/29 -j DROP
iptables -I INPUT -s 140.205.201.32/28 -j DROP
iptables -I INPUT -s 140.205.225.192/29 -j DROP
iptables -I INPUT -s 140.205.225.200/30 -j DROP
iptables -I INPUT -s 140.205.225.184/29 -j DROP
iptables -I INPUT -s 140.205.225.183/32 -j DROP
iptables -I INPUT -s 140.205.225.206/32 -j DROP
iptables -I INPUT -s 140.205.225.205/32 -j DROP
iptables -I INPUT -s 140.205.225.195/32 -j DROP
iptables -I INPUT -s 140.205.225.204/32 -j DROP
```

&emsp;&emsp;推荐屏蔽云盾/24段：

```
iptables -I INPUT -s 140.205.201.0/24 -j DROP
iptables -I INPUT -s 140.205.225.0/24 -j DROP
```

## 检查阿里云盾是否卸载干净

&emsp;&emsp;最后检查一下自己的服务器上的阿里云盾是否卸载干净了，主要就是看进程里有没有阿里云盾的相关进程了(AliYunDun、aliyun-service和AliYunDunUpdate),可以通过下面的命令来检查，如果没有相关进程则说明阿里云盾已经卸载干净了。

```
ps -aux | grep -E 'aliyun|AliYunDun'
```