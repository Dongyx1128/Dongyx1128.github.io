---
title: Gitalk未找到相关的Issues进行评论---解决方法
date: 2022-05-07 22:55:20
categories: 
- 博客
- Gitalk
tags: 
- 博客
---

&emsp;&emsp;搭建博客的时候用gitalk作为评论框架，结果配置完成后，在进行测试的时候发现出现以下问题：

> 未找到相关的Issues进行评论，请联系XXX进行创建。

&emsp;&emsp;登录之后发现评论功能也无法显示。

&emsp;&emsp;以下为解决方法：

&emsp;&emsp;刚开始使用默认网址，gitalk可以正常使用，后面因为自己使用了域名，所以导致gitalk初始化出现异常。

<!-- more -->

1. 打开github的`settings` --->`developer settings`--->`OAuth application gitalk`应用的设置。

2. Homepage URL设置为 **username.github.io**，精确到https的地址。

3. Authorization callback URL直接填写 **`注册的域名`** 就可以，注意也要精确到https。

4. update之后进行正常登录github即可评论。



​		**Gitalk注册完整信息填写如下：**
```
Application name:           # 随便填写
Homepage URL:               # 填写博客所在的仓库名。比如填写: http://Dongyx1128.github.io
Application description:    # 可以不填写
Authorization callback URL: # 填写域名。比如:https://dyx1128now.online/;如果没有就填写仓库名即可.
```
&emsp;&emsp;点击`Register Application`就可以创建
		&emsp;&emsp;接着就可以看到该应用的`Client ID`和`Client Secret`。