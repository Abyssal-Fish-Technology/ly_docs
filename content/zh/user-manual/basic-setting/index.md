---
title: 基础配置
linkTitle: 基本配置
description: 安装后，首先进行一些基本配置，使系统更好的运行.
date: 2022-12-29
publishdate: 2022-12-29
categories: [usage,fundamentals]
keywords: [usage,setting,config]
menu:
  docs:
    parent: user-manual
    weight: 25
toc: true

aliases: [/content/basic-setting/]
isCJKLanguage: true
---

## 前提条件
通过浏览器访问`http://yourserver:18080/ui`，在登录页面使用默认管理员账号admin账号登录，登录页面如下图所示。


## 黑白名单设置
为了减少出现误报、漏报的情况，用户需要准备黑白名单设备。
* 白名单主要是外发类设备：漏扫设备，此类设备本身就会触发扫描类事件。
* 黑名单则是企业积累的恶意IP，系统会检测黑名单通讯，并产生安全事件告警。

在配置菜单下，选择黑白名单子菜单进行配置
![线索追踪](/img/ly_event_config.png)


配置项字段如下表所示
|名称|必填|描述|
|---|---|---|
|IP|是|黑/白名单设备IP|
|端口|否|黑/白名单设备端口|
|描述|是|描述信息|


