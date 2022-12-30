---
title: 流影架构
linktitle: 架构
description: 流影采用分布式分层数据处理与展现架构
date: 2022-12-12
publishdate: 2022-12-12
lastmod: 2022-12-21
categories: [关于流影,fundamentals]

menu:
  docs:
    parent: "about"
    weight: 20
weight: 20
sections_weight: 20
draft: false
toc: true
isCJKLanguage: true
---

## 总体架构
流影整体设计采用四层分层架构，包括数据采集层、数据分析与检测层、数据融合分析层、展示层。

![流影架构](/images/liuying/ly_structure.png)

## 层级功能
* 数据层
  - 网络镜像流量采集
  - 威胁情报IOC接入
  - 资产指纹数据
  - IP定位信息
  - 其他扩展信息

* 分析检测层
  - 数据包分析检测
  - 网络通讯行为与内容分析：扫描行为、异常网络服务、异常通讯、异常数据流动、情报碰撞
  - 多种模型：经验模型+机器学习模型

* 融合分析层
  - 行为特征关联融合
  - 告警聚合
  - 专家模型综合研判

* 展示层
  - 安全态势报表
  - 交互式调查分析
  - 网络行为追溯查看
  - 流量可视化

## 模块组成

* 流量探针[ly_probe]: 位于数据层，开发语言C/C++
  - 源自早期开源的高性能探针nprobe5.x版本，进行了定制化开发
  - 解析网络流量、提取流量关键信息，以netflow格式传输至采集器
  - 实现将流量或pcap文件解析后，提取并进行指纹匹配
* 分析引擎[ly_analyser]: 位于分析检测层，开发语言C/C++
  - 支持netflow v9格式的数据分析
  - 网络行为分析、特征提取
  - 经验模型检测、数据包检测、威胁情报检测、机器学习模型检测
  - 网络行为证据留存

* 管理引擎[ly_server]: 位于融合分析层， 开发语言C/C++
  - 安全告警聚合
  - 分析模型配置管理
  - 数据节点管理
  - 数据查询接口
  - 用户管理

* 可视化分析引擎[ly_vis]: 位于展现层，开发语言JavaScript
  - 安全态势
  - 流量数据可视化呈现
  - 安全告警、报表
  - 网络行为交互式分析

## 连接关系
流影各个模块直接的通信连接关系如下图所示。
![流影模块关系](/images/liuying/ly_mod2.png)

* 流量采集节点ly_probe
  - 支持网络流量镜像接入方式，根据流量大小可以多节点部署
  - 输出数据推送到任一分析检测引擎ly_analyser
  - 数据包留存

* 分析检测节点ly_analyser
  - 输入探针采集推送数据
  - 根据流量情况，支持多节点横向扩展部署
  - 输出网络安全事件特征(event_features)、通讯会话特征（session features)
  - 数据存储本地节点MongoDB数据库

* 管理服务节点ly_server
  - 单节点部署即可
  - 内建多种网络行为分析模型，灵活，支持自定义配置模型
  - 提取分析节点网络行为特征数据，进行融合与关联分析，输出聚合安全事件告警，存储在本地MySQL（MariaDB）数据库
  - 提供各层数据节点查询接口

* 可视化呈现与交互式分析节点ly_vis
  - 单节点部署即可
  - react框架、h5+js、d3.js
  - 支持用户对流量的可视化分析、交互式探索、挖掘与追溯
  - 图表丰富，简洁易用


[ly_probe]:https://github.com/Abyssal-Fish-Technology/ly_vis
[ly_analyser]:https://github.com/Abyssal-Fish-Technology/ly_analyser
[ly_server]:https://github.com/Abyssal-Fish-Technology/ly_server
[ly_vis]:https://github.com/Abyssal-Fish-Technology/ly_probe