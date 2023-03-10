---
title: 态势总览
linkTitle: 态势总览
description: 宏观总览安全态势、全局数据检索、系统运行状态管理.
date: 2022-12-29
publishdate: 2022-12-29
categories: [usage,fundamentals]
keywords: [usage,zonglan,event]
menu:
  docs:
    parent: user-manual
    weight: 30
toc: true

aliases: [/content/usage/zonglan]
isCJKLanguage: true
---

## 子菜单
总览分为运维、管理、分析三个子模块，为运维人员提供全方位、可视化的安全监控以及系统管理服务。

## **运维**
流影【总览/运维】页面，从事件分类统计、Top10受害目标事件分布、事件时间趋势以及多维统计排行等四个统计维度去分析安全事件。并提供工作台模块方便运维人员能够快速处理安全事件。

### **控制栏**
运维页面的数据范围可以通过操作导航下方的控制栏来进行控制。

![控制](/img/usage/Aspose.Words.4325b89e-3d42-4255-924b-c68f9f0e7d81.006.png)


运维页面的顶部操作栏包含以下模块。

|模块|描述|
| - | - |
|自动刷新|选中后每五分钟自动更新运维页面数据。默认选中。|
|资产事件按钮|选中后仅展示所配置资产相关的安全事件。默认选中。|
|时间选择器|选取时间范围，提供今日、近三天、近一周的快捷选中方式。|
|采集节点|切换观察的流量环境。|
|刷新按钮|刷新页面。|

### **分类统计**
分类统计包含展示视图与设置视图两个视图。
![](/img/usage/Aspose.Words.4325b89e-3d42-4255-924b-c68f9f0e7d81.007.png)


![](/img/usage/Aspose.Words.4325b89e-3d42-4255-924b-c68f9f0e7d81.008.png)

#### **告警类型排序**
用户可以将自己关注的告警类型放在更显眼的位置。

1. 点击“展示视图右侧的“设置”按钮，出现“设置视图”。
1. 用鼠标将“展示”区域的类型卡片拖拽排序。
1. 点击“关闭”按钮或按“esc”键关闭弹窗。

#### **忽略某类告警**
用户可以长期在运维页面隐藏某类不关注的告警。

1. 点击“展示视图”右侧的“设置”按钮，出现“设置视图”。
1. 用鼠标将“展示”区域的目标类型卡片拖拽至“隐藏”区域内。
1. 点击“关闭”或按“esc”键关闭弹窗。

#### **隐藏某类告警**
当用户不想或者仅想通过运维页面视角查看某几类安全事件的分布、统计情况时，可以把本类型或者其他类型的事件隐藏掉。

1. 点击展示视图类型类型卡片左上角的“复选框”取消选中，即可隐藏该类型的安全事件
1. 点击“复选框”选中，即可展示该类型。

#### **展示全部类别**
事件类型太多的时候，界面展示不下，会有部分告警类型隐藏起来。可以通过点击“展示视图”右侧的“更多”按钮。


### **Top10受害目标事件分布**
#### **图介绍**
展示安全事件中受影响设备攻击数量的Top10安全事件，并以可视化的形式按照攻击源、事件类型、受害目标、受害方所属资产组等多维度去展示它们之间的关联。

用户可以从本模块得到关于受攻击最多的设备的一系列安全事件信息，例如属于哪些资产组、受到了来自哪些设备的哪种攻击、攻击设备之间有无关联等。

![](/img/usage/Aspose.Words.4325b89e-3d42-4255-924b-c68f9f0e7d81.016.png)


![](/img/usage/Aspose.Words.4325b89e-3d42-4255-924b-c68f9f0e7d81.017.png)

#### **图操作**
1. 单维度排序：分布图的单个维度，默认按照从发生事件数量从大调整顺序，可以通过点击维度标注右侧的“排序”按钮，进行正序或者倒序排列，有时能起到图形更规整，更容易寻找关联关系。
1. 多维度排序：可以通过拖拽任意维度标注如“受害目标”，向左或者向右进行维度之间排序。能够更好的观察关注的两个维度之间的关联联系。
1. 选择展示维度：点击右上角“显示维度”按钮，选择或取消选择展示的维度。
1. 刷新：点击右上角“刷新”按钮，能够让视图快速恢复到初始状态。
1. 展示关联：将鼠标移动到观察条目所处的柱状图上。相关的元素会着重展示出来。
1. 查看详情：将鼠标移动到对应的观察条目上。将会出现统计信息提示框。
1. 查看列表：将鼠标移动至统计信息提示框上，点击“查看列表”，系统会跳转至事件列表页，展示对应的安全事件。
1. 设备操作：点击设备名称，出现操作列表弹窗，选择操作类型。

### **工作台**
工作台仅展示还未处理的安全事件，运维人员可以通过工作台去完成今日工作。通过点击ID列去查看事件详细信息，然后对本次事件进行研判。

工作台
![](/img/usage/Aspose.Words.4325b89e-3d42-4255-924b-c68f9f0e7d81.021.png)


### **事件时间趋势**
展示时间范围内每5分钟的事件发生量的趋势。
![](/img/usage/Aspose.Words.4325b89e-3d42-4255-924b-c68f9f0e7d81.022.png)


### **多维统计排行**
多维统计模块分别从攻击设备、受害设备、受害资产组三个用户最关心的维度去统计排行。用户可以快速了解哪些设备告警最频繁、主要受害区域等信息。

![](/img/usage/Aspose.Words.4325b89e-3d42-4255-924b-c68f9f0e7d81.023.png)


## **管理**
流影【总览/管理】页面，分为配置总览、系统状态、事件规则三个功能模块。能够让用户快速了解系统配置内容、系统运行状态、以及优化检出规则。
### **系统配置总览**
配置总览中包含部署设备数量、识别安全事件规则、资产组配置以及用户管理四个方面配置数量。
### **系统运行状态**
系统状态是将系统的各层设备节点以可视化的形式呈现出来，方便用户查看在多节点部署复杂网络环境中的关联关系。可以查看设备的运行状态、内存情况、各个进程是否运行，并且提供进程的开关、设备启动等多种操作。

![](/img/usage/Aspose.Words.4325b89e-3d42-4255-924b-c68f9f0e7d81.024.png)

### **事件规则有效性**
展示事件规则最近一周的检出趋势，用户可以由此判断规则的有效性。例如当一个规则的检出量特别大的时候，需要查看对应的事件，综合考虑是否需要提高规则的检测规则；当一个规则的检出量特别小的时候，结合实际环境，综合考虑是否需要降低规则的检测规则。

![](/img/usage/Aspose.Words.4325b89e-3d42-4255-924b-c68f9f0e7d81.025.png)


## **分析**
分析页面为用户提供了全局搜索功能，通过指定的查询条件，查询目标在系统内的所有信息，包括信息总览、威胁情报信息、安全事件、特征行为等信息。

通过切换查询类型来实现IP、端口、域名、IP 端口、IP>端口以及IP：端口六种类型的目标查询。其中类型的含义如下。

- IP：IP地址
- 端口：端口号
- 域名：域名
- IP 端口：指定IP和端口，但是不指定方向，包含SIP-DPORT、SIP-SPORT、SPORT-DIP以及DIP-DPORT四种情况。
- IP>端口：SIP-DPORT的组合
- IP:端口：SIP-SPORT的组合

![](/img/usage/Aspose.Words.4325b89e-3d42-4255-924b-c68f9f0e7d81.026.png)

### **信息总览**
信息总览会展示设备基础信息、安全事件、行为特征三个方面的统计信息。

- 基础信息：地理位置、经纬度、运营商、设备标签等信息。
- 事件信息：在指定时间范围内的安全事件统计信息，包括作为攻击方和受害方的统计信息以及安全事件的分布。
- 特征统计信息：六种行为特征统计数据。六种数据的具体含义会在[行为特征](#_行为特征)部分描述。

![](/img/usage/Aspose.Words.4325b89e-3d42-4255-924b-c68f9f0e7d81.027.png)


### **威胁情报**
威胁情报展示设备的威胁情报详细信息，包括情报标签、情报数量、更新时间、威胁评分等信息，含义如下：

- 情报标签：最新的情报分类。
- 情报数据：情报数据的离散数量。
- 最早发现时间：情报数据第一次标记的时间。
- 最新更新时间：情报数据在情报库中最新更新时间。

情报的详细信息分为来源聚合、标签聚合以及时间线三个维度来展示，可以通过对比不同来源对查询目标的不同标记来辅助用户做决策。

![](/img/usage/Aspose.Words.4325b89e-3d42-4255-924b-c68f9f0e7d81.028.png)

### **事件列表**
安全事件列表则是展示查询目标在指定时间范围内的安全事件列表。支持各维度的排序、查看详情以及数据导出操作。

![](/img/usage/Aspose.Words.4325b89e-3d42-4255-924b-c68f9f0e7d81.029.png)

### **行为特征**
系统会记录设备的六种行为特征，并做了不同维度的统计分析。可以用来分析设备属性、发现异常行为、调查和溯源安全事件等。
#### **风险通讯**
目标设备和威胁情报中记录的威胁设备发生通讯的风险行为。从下图中我们可以看到10.6.8.101建立了和98.137.159.25（木马主控网站）的通讯，

![](/img/usage/Aspose.Words.4325b89e-3d42-4255-924b-c68f9f0e7d81.030.png)


#### **黑名单通讯**
目标设备的黑名单通讯的行为。会记录通讯的双方设备、通讯时间、协议、产生的流量大小等信息。
#### **扫描通讯**
扫描源或者被扫端口涉及的扫描行为，会记录扫描源、端口、协议、产生的流量大小等信息。
#### **TCP通讯**
目标设备的建立TCP连接的动作，包括主动发起TCP请求或者接收TCP请求，从而判断在一次连接中，目标设备是作为客户端还是服务端。在异常行为分析和威胁溯源时，可以判断是哪一方是主动方。

例如从下图中，我们可以看到是10.6.8.101这个内网设备在2022-12-27 16：44：53主动请求了98.137.159.25这个美国IP，请求了共100次，共产生1.18M字节的流量，十分可疑。

![](/img/usage/Aspose.Words.4325b89e-3d42-4255-924b-c68f9f0e7d81.031.png)

#### **服务记录**
服务记录是指查询目标在指定时间内提供或者请求的服务记录。并且记录动作时间、服务端口、服务是否响应、产生流量等。

从下图中我们可以看到98.137.159.25有一个活跃服务端口25。并且在2022-12-27 16:44:53收到了请求，并及时的给出了响应。结合TCP通讯行为，我们可以得到10.6.8.101的请求成功了。

![](/img/usage/Aspose.Words.4325b89e-3d42-4255-924b-c68f9f0e7d81.032.png)

#### **DNS查询记录**
记录查询目标在指定时间范围内的DNS查询记录，并且记录查询内容，DNS服务器地址、查询类型、是否为威胁域名以及产生流量大小等信息。

用户可以从DNS查询记录中得到大量的设备信息。例如从该设备近500条DNS查询记录中，我们可以得到这是一个客户端机器，系统环境是windows，装有飞书、微信、QQ、腾讯会议等一系列软件，是一名原神玩家。

![](/img/usage/Aspose.Words.4325b89e-3d42-4255-924b-c68f9f0e7d81.033.png)
