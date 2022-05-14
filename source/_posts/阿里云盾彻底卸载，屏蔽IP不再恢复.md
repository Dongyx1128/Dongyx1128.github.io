---
title: 阿里云盾彻底卸载，屏蔽IP不在恢复
date: 2020-10-06 08:05:20
categories: 
- 服务器
- 卸载云盾
tags: 
- 服务器
- 阿里云
- 腾讯云
---
# 1. 关于云盾/云镜(阿里云盾/腾讯云盾)

## - 阿里云盾
&emsp;&emsp;用过阿里云ecs服务器或者产品的人都知道，阿里云是自带安全扫描还有云盾防御的，虽然表面上是说给你安全防御，其实就是为了获取你的数据。

&emsp;&emsp;另外还可以记录一些攻击记录，让你付费解决，不过都没什么卵用，该被攻击的还是会被攻击，该花钱解决问题的一个子都少不了。

&emsp;&emsp;如果自己服务器天天要被阿里云扫描，那么有什么违规关键词都会给你封了，特别是小站，没人气就没攻击，不过阿里云会天天攻击你，告诉你网站漏洞。

&emsp;&emsp;最后呢，要么你自己解决，要么你就花钱解决，对于强迫症的人来说，阿里云盾给你带来的是无时无刻的折腾。

## -  腾讯云镜
&emsp;&emsp;云监控 Linux 安装目录是 /usr/local/qcloud/stargate 和 /usr/local/qcloud/monitor，同时还有主机安全，也即云镜，新开服务器不取消勾选都会默认安装。
<!-- more -->

# 2. 卸载云盾

## 2.1. 卸载阿里云盾

&emsp;&emsp;远程连接到阿里云云服务器或者轻量应用服务器后，执行以下代码卸载阿里云盾：

```
wget http://update.aegis.aliyun.com/download/uninstall.sh
chmod +x uninstall.sh
./uninstall.sh
wget http://update.aegis.aliyun.com/download/quartz_uninstall.sh
chmod +x quartz_uninstall.sh
./quartz_uninstall.sh
```

### - (1)删除阿里云盾文件残留

&emsp;&emsp;卸载阿里云盾后，执行如下代码删除阿里云盾文件残留：

```
pkill aliyun-service
rm -fr /etc/init.d/agentwatch /usr/sbin/aliyun-service
rm -rf /usr/local/aegis*
```

### - (2)屏蔽阿里云盾IP

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

### - (3)检查阿里云盾是否卸载干净

&emsp;&emsp;最后检查一下自己的服务器上的阿里云盾是否卸载干净了，主要就是看进程里有没有阿里云盾的相关进程了(AliYunDun、aliyun-service和AliYunDunUpdate),可以通过下面的命令来检查，如果没有相关进程则说明阿里云盾已经卸载干净了。

```
ps -aux | grep -E 'aliyun|AliYunDun'
```

## 2.2 卸载腾讯云镜

直接使用腾讯云自带的卸载脚本卸载。
### - (1)脚本卸载
```
/usr/local/qcloud/stargate/admin/uninstall.sh
/usr/local/qcloud/YunJing/uninst.sh
/usr/local/qcloud/monitor/barad/admin/uninstall.sh
```
### - (2)检查卸载后是否存在文件残留：
```
ps -A | grep agent
```
如无任何输出，则已卸载干净，如果有输出，请检查是否你自己的程序。

# 3. 使用一键脚本(只适用于阿里云盾)

&emsp;&emsp;阿里云盾卸载步骤居多，推荐使用一键脚本进行卸载(以下脚本选择其一即可)：
```
wget -N --no-check-certificate https://raw.githubusercontent.com/babywbx/Uninstall-aliyun-service/master/UAS.sh && chmod 777 UAS.sh && ./UAS.sh
```
```
wget --no-check-certificate https://raw.githubusercontent.com/duck356/removeAliYunDun/master/removeyundun.sh
chmod +x removeyundun.sh
./removeyundun.sh
```
```
curl -sSL https://cdn.jsdelivr.net/gh/HXHGTS/AliyunProtectUninstall/TPU.sh | sh
```