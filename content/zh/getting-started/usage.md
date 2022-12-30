---
title: 用户界面
linktitle: 界面使用
description: 简单介绍流影可视化界面基本操作。
date: 2022-12-29
publishdate: 2022-12-29
categories: [getting started, fundamentals]
keywords: [usage]
menu:
  docs:
    parent: "getting-started"
    weight: 40
weight: 40
sections_weight: 40
aliases: [/overview/usage/,/extras/livereload/,/doc/usage/,/usage/]
toc: true
isCJKLanguage: true
---
## 前言
您完成流量各个节点的安装配置后，就可以通过前端界面进行操作和使用了。
如果您没有本地安装，也可以使用我们提供的线上演示Demo。

## 演示环境
您可以使用以下Demo快速体验流影功能

- [Demo地址](http://101.254.236.75:12280/ui/#/login)
- 演示账号：demo，口令：demo@2022


## 登录
访问系统地址，进入登录界面。使用合法账号登录系统。
![用户登录](/img/login.png)

## 态势总览
进入系统后，展示的是态势总览界面
![态势总览](/img/zong.png)

流影【总览/运维】页面，从事件分类统计、Top10受害目标事件分布、事件时间趋势以及多维统计排行等四个统计维度去分析安全事件。并提供工作台模块方便运维人员能够快速处理安全事件。


## 全局搜索
在总览菜单下，选择 `分析` 子菜单，进入全局搜索
![全局搜索](/img/ly_search.png)

分析页面为用户提供了全局搜索功能，通过指定的查询条件，查询目标在系统内的所有信息，包括信息总览、威胁情报信息、安全事件、特征行为等信息。

通过切换查询类型来实现IP、端口、域名、IP 端口、IP>端口以及IP：端口六种类型的目标查询。其中类型的含义如下。
* IP：IP地址
* 端口：端口号
* 域名：域名
* IP 端口：指定IP和端口，但是不指定方向，包含SIP-DPORT、SIP-SPORT、SPORT-DIP以及DIP-DPORT四种情况。
* IP>端口：SIP-DPORT的组合
* IP:端口：SIP-SPORT的组合

## 系统管理
流影【总览/管理】页面，分为配置总览、系统状态、事件规则三个功能模块。能够让用户快速了解系统配置内容、系统运行状态、以及优化检出规则。

![系统管理](/img/ly_config.png)

## 事件告警
流影【事件】页面是用来查看安全事件列表的，支持安全事件的查询、过滤、统计、处理等操作。可以通过导航进入此页面，也可以通过设备操作菜单中的`事件列表`进入到事件页面。

![事件告警](/img/event.png)

事件告警列表
![事件告警](/img/ly_elist.png)

## 告警详情
在事件告警列表右侧ID列，点击告警事件id，可查看该告警汇总详细信息。
![事件告警信息](/img/event_detail.png)


## 线索追踪
用户可以使用追踪功能对 IP、端口、会话关系等目标进行持续关注、标签备注、分组管理以及自定义告警。追踪页面针对于追踪目标提供查询筛选、追踪列表展示、安全事件关联、资产信息关联等功能。
![线索追踪](/img/ly_zhuizong.png)

## 系统配置
系统配置包含了事件检测策略、资产范围、黑白名单配置、追踪目标设置、用户管理等。

![线索追踪](/img/ly_event_config.png)

## 其它
更详细使用请参见用户[手册内容](/user-manual/)。

## 联系反馈

- 邮箱：opensource@abyssalfish.com.cn

