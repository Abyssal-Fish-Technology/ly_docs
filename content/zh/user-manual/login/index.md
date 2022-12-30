---
title: 用户登录
linkTitle: 登录系统
description: 需要登录系统后，才能使用各项功能.
date: 2022-12-29
publishdate: 2022-12-29
categories: [usage,fundamentals]
keywords: [usage,login,user]
menu:
  docs:
    parent: user-manual
    weight: 20
toc: true
weight: 20
aliases: [/content/user-login/]
isCJKLanguage: true
---

## 前提条件
用户成功安装部署各个节点组件后，可以访问系统界面。
界面访问地址一般是 `http://yourserver:18080/ui`。

您也可以使用以下Demo快速体验流影功能

- [Demo地址](http://101.254.236.75:12280/ui/#/login)
- 演示账号：demo，口令：demo@2022

## 用户登录
通过浏览器访问`http://yourserver:18080/ui`，在登录页面使用默认admin账号登录，登录页面如下图所示。

![用户登录](/img/login.png)

输入用户名和登录密码。

## 用户权限
管理员账号具有最高权限，可以进行系统所有配置和用户账号管理操作。
系统角色目前设定为：
* 管理员：具有所有权限
* 分析员：可以配置检测规则、设置告警
* 普通用户：只有查看权限

{{% note %}}
演示账号demo为普通用户。
{{% /note %}}


