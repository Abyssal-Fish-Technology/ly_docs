---
title: 快速上手
linktitle: 快速上手
description: 了解流影节点部署、配置、运行方式
date: 2022-12-22
publishdate: 2022-12-23
categories: [deploy,getting started, fundamentals]
keywords: [quick start,usage]
menu:
  docs:
    parent: getting-started
    weight: 10
weight: 10
toc: true
aliases: [/quickstart/,/overview/quickstart/]
isCJKLanguage: true
---

通过本文内容，您将了解到:

1. 流影各模块节点部署介绍
2. 部署架构
3. 简单使用

## 准备

在开始部署之前，您必须准备好部署条件:

1. [硬件环境](/getting-started/deploy-required/#硬件环境)
1. [软件环境](/getting-started/deploy-required/#软件环境)
1. [网络环境](/getting-started/deploy-required/#网络环境)

相关环境要求请参考[部署环境](/getting-started/deploy-required).

## 节点架构
流影包括四个主要模块，分别是
* 采集模块 ly_probe：可以单节点部署，也支持多节点横向扩展部署（提高采集性能）
* 分析模块 ly_analyser：可以单节点部署，支持多节点横向扩展部署（提高分析性能）
* 管理模块 ly_server：单节点部署即可
* 可视化模块 ly_vis：单节点部署即可

节点部署示意图
![节点部署](/images/liuying/ly_nodes.png)

根据实际流量大小，灵活设置节点数量
* 最小化部署：单节点部署所有模块，适合小流量测试
* 分布式部署：多流量采集节点，多分析节点，一个管理节点和可视化节点，适合大流量情况


## 探针使用

**流影探针 ly_probe** 是一款用于解析网络流量、提取关键信息的探针软件，源自nprobe早期5.x开源版本，
是一款高性能流量采集软件。ly_probe基于nprobe5.x进行了定制化开发，能够将流量或pcap文件解析后，提取并匹配指定数据内容后，以netflow格式传输至采集器。
除提取网络层相关信息，亦可解析HTTP、DNS、ICMP等应用层协议。
通过配置指纹规则库，支持提取应用层协议相关应用、设备、操作系统等绑定到IP详细扩展信息，也支持识别应用层的威胁行为，或其他匹配目标特征的网络行为。
规则命中的数据包可以根据需求以文件方式同步留存，供后续调查验证使用。

**流量探针节点部署位置**

| 流量节点位置          | 危险程度 | 威胁倾向                                                     |
| --------------------- | -------- | ------------------------------------------------------------ |
| 互联网-终端用户       | 高危     | 关注终端用户的上网安全问题，通过此节点流量识别用户与威胁来源的通信、判断是否受到远程控制，检测主机是否失陷。 |
| 互联网-DMZ区域        | 高危     | 关注互联网中面向企业对外服务器的威胁行为。                   |
| 互联网-内部服务器     | 高危     | 关注内部服务器对外暴露情况，关注内部服务器数据流出情况，检测服务器是否失陷 |
| 内部服务器-终端用户   | 危险     | 关注终端用户对服务器的攻击行为，检测异常终端用户，检测服务器所受到的威胁。 |
| 内部服务器-内部服务器 | 危险     | 关注服务器间横向攻击行为，检测失陷服务器与服务器所受到的威胁。 |


#### ly_probe 安装
您可以从项目地址下载realse进行安装
* [Github: ly_probe release](https://github.com/Abyssal-Fish-Technology/ly_probe/releases/)
* [Gitee: ly_probe release](https://gitee.com/abyssalfish-os/ly_probe/releases/)

####  运行示例

​以守护进程形式在后台运行，监听eth0网卡的数据流量，解析MAC、IP、L4协议、端口、数据量等基础信息，以netflow形式发送至127.0.0.1:9995。
```sh
# lyprobe -T "%IN_SRC_MAC %OUT_DST_MAC %IPV4_SRC_ADDR %IPV4_DST_ADDR %PROTOCOL %L4_SRC_PORT %L4_DST_PORT %TCP_FLAGS %SRC_TOS %IN_PKTS %IN_BYTES" -n 127.0.0.1:9995 -e 0 -w 32768 -G -i eth0
```



#### 运行参数

通过参数可以指定数据源、运行模式、解析格式、采集器位置等。
```text
# lyprobe -h
lyprobe -n <host:port|none> [-i <interface|dump file>] [-f <filter>]
    [-G] [-O <# threads>] [-w <hash size>] [-e <flow delay>]
        [-T <flow template>] [-U <flow template id>]
            ......
            [--collector|-n] <host:port|none>   | Address of the NetFlow collector(s).
            | Multiple collectors can be defined using
            | multiple -n flags. In this case flows
            | will be sent in round robin mode to
            | all defined collectors if the -a flag
            | is used. Note that you can specify
            | both IPv4 and IPv6 addresses.
            | If you specify none as value,
            | no flow will be export; in this case
            | the -P parameter is mandatory.
            [--interface|-i] <iface|pcap>       | Interface name from which packets are
            | captured, or .pcap file (debug only)
            [--bpf-filter|-f] <BPF filter>      | BPF filter for captured packets
                | [default=no filter]
                [--daemon-mode|-G]                  | Start as daemon.
                [--num-threads|-O] <# threads>      | Number of packet fetcher threads
                | [default=2]. Use 1 unless you know
                | what you're doing.
                [--hash-size|-w] <hash size>        | Flows hash size [default=32768]
                    [--flow-delay|-e] <flow delay>      | Delay (in ms) between two flow
                        | exports [default=1]
                        [--flow-templ|-T] <flow template>   | Specify the NFv9 template (see below).
                            --pcap-file-list <filename>         | Specify a filename containing a list
                                | of pcap files.
                                | If you use this flag the -i option will be
                                | ignored.
                                ......

```


#### 插件功能

##### 服务识别插件 - servicePlugin

​通过数据包传输层载荷进行正则模式匹配，结合资产指纹规则，识别数据流应用层协议，目标IP的中间件服务、设备种类、操作系统等资产信息；结合威胁指纹规则，识别指定类型的威胁流量。

​指纹规则存放于 **fp-pattern/** 目录中，包含以JSON格式书写的五类规则：

```text
service.json	-- 应用层协议与服务识别规则
​device.json	-- 硬件设备识别规则
​os.json		-- 操作系统识别规则
​midware.json	-- 软件平台与中间件规则
​threat.json	-- 异常流量识别规则
```



​指纹规则通过数个字段指定匹配条件与标记结果，例如：
```text
{
    "midware":{
        "rules":{
            "40001":{
                "type":"Web Service",	-- 命中后类型标识，输出至XXX_TYPE字段
                "name":"Nginx",			-- 命中后名称标识，输出至XXX_NAME字段
                "protocol":"tcp",		-- 指定传输层协议
                "port":"80",			-- 指定端口
                "is_http":1,			-- 指定是否为HTTP流量
                "regex":"Server: nginx(?:\\/(\\d*\\.?\\d*\\.?\\d*))?",	-- 正则语句
                // "version":""			-- 使用指定字符串声明版本信息；未设置字段时,
                //						-- 提取第一个正则子表达式命中内容作为版本信息
            }
        }
    }
}

```



​指定字段，在采集数据过程中进行规则匹配，获取到服务、设备、操作系统、中间件的类型、名称、版本等信息以及威胁流量的描述信息。插件留存规则命中的数据包，并通过微秒级的时间戳，对指定数据包进行定位查找。

```sh
# lyprobe -T "%IN_SRC_MAC %OUT_DST_MAC %IPV4_SRC_ADDR %IPV4_DST_ADDR %PROTOCOL %L4_SRC_PORT %L4_DST_PORT %TCP_FLAGS %SRC_TOS %IN_PKTS %IN_BYTES %SRV_TYPE %SRV_NAME %SRV_VERS %DEV_TYPE %DEV_NAME %DEV_VEND %DEV_VERS %OS_TYPE %OS_NAME %OS_VERS %MID_TYPE %MID_NAME %MID_VERS %THREAT_TYPE %THREAT_NAME %THREAT_VERS %SRV_TIME %DEV_TIME %OS_TIME %MID_TIME %THREAT_TIME" -n 127.0.0.1:9995 -e 0 -w 32768 -G -i eth0
```



##### DNS协议解析插件 - dnsPlugin

​解析PCAP文件的数据流量，实现对DNS协议内容的解析，获取DNS查询域名，查询类型，查询结果关联IP。

```sh
# lyprobe -T "%IN_SRC_MAC %OUT_DST_MAC %IPV4_SRC_ADDR %IPV4_DST_ADDR %PROTOCOL %L4_SRC_PORT %L4_DST_PORT %TCP_FLAGS %SRC_TOS %IN_PKTS %IN_BYTES %DNS_REQ_DOMAIN %DNS_REQ_TYPE %DNS_RES_IP" -n 127.0.0.1:9995 -e 0 -w 32768 -G -i eth0
```


##### HTTP协议解析插件 - httpPlugin

​解析PCAP文件的数据流量，实现对HTTP协议内容的解析，获取HTTP协议头部字段信息。

```sh
# lyprobe -T "%IN_SRC_MAC %OUT_DST_MAC %IPV4_SRC_ADDR %IPV4_DST_ADDR %PROTOCOL %L4_SRC_PORT %L4_DST_PORT %TCP_FLAGS %SRC_TOS %IN_PKTS %IN_BYTES %HTTP_URL %HTTP_REQ_METHOD %HTTP_HOST %HTTP_MIME %HTTP_RET_CODE %HTTP_USER_AGENT" -n 127.0.0.1:9995 -e 0 -w 32768 -G -i eth0
```

##### ICMP协议解析插件 - icmpPlugin

​解析PCAP文件的数据流量，实现对ICMP协议内容的解析，获取ICMP载荷内容等信息。

```sh
# lyprobe -T "%IN_SRC_MAC %OUT_DST_MAC %IPV4_SRC_ADDR %IPV4_DST_ADDR %PROTOCOL %L4_SRC_PORT %L4_DST_PORT %TCP_FLAGS %SRC_TOS %IN_PKTS %IN_BYTES  %ICMP_DATA %ICMP_SEQ_NUM %ICMP_PAYLOAD_LEN" -n 127.0.0.1:9995 -e 0 -w 32768 -G -i eth0
```

## 分析引擎使用
ly_analyser是流影的网络行为分析引擎，读取netflow v9格式的数据作为输入，运行各种威胁行为检测模型，产出威胁事件，并留存相关特征数据用于后续取证分析。包括扫描、DGA、DNS隧道、ICMP隧道、服务器外联、 挖矿、各种注入等威胁行为，涵盖机器学习、威胁情报、数据包检测、经验模型四种识别方式。

#### ly_analyser 安装
您可以从ly_analyser项目地址下载realse进行安装
* [Github: ly_analyser release](https://github.com/Abyssal-Fish-Technology/ly_analyser/releases/)
* [Gitee: ly_analyser release](https://gitee.com/abyssalfish-os/ly_analyser/releases/)

您也可以安装文档进行源码编译安装。

#### ly_analyser 配置

##### 系统环境配置
```
#  配置环境语言及时区
export LANG=en_US.UTF-8
ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
ntpdate cn.pool.ntp.org

# 关闭seliunx，开放本地防⽕墙端口
#编辑config⽂件
vi /etc/selinux/config
#找到配置项
SELINUX=enforcing
#修改配置项为
SELINUX=disabled

#执⾏命令，即时关闭selinux
setenforce 0

#开放本地防⽕墙端口
systemctl restart firewalld
firewall-cmd --zone=public --add-port=10081/tcp --permanent
firewall-cmd --reload
```

######  httpd配置
编辑文件`/etc/httpd/conf.d/agent.conf`，写入内容：
```text
Listen 10081
<VirtualHost *:10081>
    DocumentRoot /Agent/cmd
    <Directory "/Agent/cmd">
        Options ExecCGI
        SetHandler cgi-script
        AllowOverride None
        Order allow,deny
        Allow from all
        Require all granted
    </Directory>
</VirtualHost>
```

#重启httpd
```sh
systemctl restart httpd
```

#### 设置任务
##### 创建定时任务
```text
# vi /var/spool/cron/apache，加入内容：
*/5 * * * * /Agent/bin/extractor
```

##### 启动nfcapd接收探针发送的netflow数据
```sh
/Agent/bin/nfcapd -w -D -l /data/flow/3 -p 9995
```

## 管理引擎使用
ly_server是流影的管理引擎，用于聚合分析引擎产出的威胁事件、数据节点管理、用户管理、配置管理、数据查询等。

#### ly_server 节点安装
您可以从 ly_server 项目地址下载 realse 进行安装
* [Github: ly_server release](https://github.com/Abyssal-Fish-Technology/ly_server/releases/)
* [Gitee: ly_server release](https://gitee.com/abyssalfish-os/ly_server/releases/)

您也可以安装文档进行源码编译安装。

#### ly_server节点运行配置

##### 系统环境配置
```
#  配置语言和时区
export LANG=en_US.UTF-8
ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime>/dev/null 2>&1
ntpdate cn.pool.ntp.org

# 关闭selinux、开启防火墙端口
#编辑config⽂件
vi /etc/selinux/config
#找到配置项
SELINUX=enforcing
#修改配置项为
SELINUX=disabled
#执⾏命令，即时关闭selinux
setenforce 0

systemctl restart firewalld
firewall-cmd --zone=public --add-port=18080/tcp --permanent
firewall-cmd --reload
```
######  httpd配置
编辑`etc/httpd/conf.d/server.conf`
```
Listen 18080
<VirtualHost :18080>
    DocumentRoot "/Server/www"
    AddDefaultCharset utf-8
    <Directory "/Server/www">
        Options FollowSymLinks Includes
        XBitHack on
        AllowOverride None
        Order allow,deny
        Allow from all
        Require all granted
    </Directory>
    Alias /d/ "/Server/www/d/"
    <Directory "/Server/www/d/">
        Options ExecCGI FollowSymLinks
        SetHandler cgi-script
        AllowOverride None
        Order allow,deny
        Allow from all
        Require all granted
        RewriteEngine On
        RewriteCond %{REQUEST_FILENAME} !auth$
        RewriteRule ^(.)$ auth?auth_target=$1 [QSA,PT,L]
    </Directory>
</VirtualHost>
```
httpd配置完成后，重启httpd

```sh
systemctl restart httpd
```
配置mariadb
```sh
mkdir -p /Server/etc

#编辑/Server/etc/gl.server.cnf⽂件
vi	/Server/etc/gl.server.cnf
```
添加如下内容
```text
[gl.server]
default-character-set=utf8
user=root
database=server
passwd=
```

复制配置文件到指定位置
```sh
cp /Server/etc/gl.server.cnf /etc/my.cnf.d/
```

运行mariadb数据库
```sh
systemctl	start	mariadb
#初始化数据库，根据提示操作即可（⽆登录密码，拒绝⾮本地登录）
mysql_secure_installation
#登陆本地数据库
mysql  -uroot
#新建数据库server
create	database	server;
#选择server数据库
use	server
#导⼊数据
source	/root/db.server.v1.0.0.clean.sql
#导⼊成功后，退出
exit
#重启mariadb
systemctl  restart  mariadb

```

#### ly_server运行配置
创建定时任务,编辑`/var/spool/cron/root`，写入内容：
```text
*/5 * * * * /Server/bin/config_pusher
*/5 * * * * /Server/bin/gen_event
```

## 可视化分析引擎使用
ly_vis是流影的前端可视化分析引擎，采用react框架、h5+js、d3.js，图表丰富，简洁易用。

#### ly_vis 节点安装
您可以从 ly_vis项目地址下载 realse 进行安装
* [Github: ly_vis release](https://github.com/Abyssal-Fish-Technology/ly_vis/releases/)
* [Gitee: ly_vis release](https://gitee.com/abyssalfish-os/ly_vis/releases/)

您也可以安装文档进行源码编译安装。

#### ly_vis 运行

将下载的 `release` 或者本地打包的文件部署在服务器的 `home/Server/www/ui`文件路径下。

部署完成后，使用Chrome浏览器访问 `your server:18080/ui` ，如出现登录页面则部署成功。

#### 配置服务器信息。

打开 `/packages/std/config-overrides.js`文件。修改 `devServerConfig`中 `target`变量为自己数据的Server路径。

```javascript
const devServerConfig = () => config => {
    return {
        ...config,
        proxy: {
            '/d/': {
                target: Your Server,
                changeOrigin: true,
            },
        },
    }
}
```

#### 本地运行

```
yarn std start
```

自动唤醒浏览器，访问[localhost:8004](localhost:8004)，展示为登录页面。如启动程序运行完毕之后未能自动访问，则需要手动访问。
