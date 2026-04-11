---
title: OM
date: 2026-03-30T10:10:15+08:00
tags: 运维something
---

接下来运维很有用...
<!-- more -->
# linux命令
#### 助信息
- man ls(查看ls手册)
- help ls(查看内置命令帮助信息)
- whatis grep(单行简要描述)
- whereis python(定位命令的二进制文件、源代码和手册页的位置)
- which ls(被执行的那个命令的绝对路径)
- info tar(超文本格式文档)
#### 目录管理
- pwd,cd,ls,tree
- mkdir,touch,rmdir(删除空目录),rm
- ln(链接),rename,mv,cp
- stat(文件详细信息),file(识别文件真实类型),chmod,chown(修改文件的组)
- locate(数据库快速找文件),find(实时遍历目录查找文件)
#### 内容查看
- cat,head,tail(尾部),more,less
- grep(文本查找)
- sed(非交互式修改文件内容),vi,vim,nano
#### 压缩解压
- tar, gzip, zip, unzip
#### 用户管理
- groupadd, groupdel, groupmod, useradd, userdel, usermod, passwd, su, sudo
#### 系统管理
- reboot, exit, shutdown, date, mount, umount, ps, kill, systemctl, service, crontab(周期性执行任务)
#### Linux 网络管理
- (curl, wget), (telnet旧版ssh, ip新版ifconfig和route, hostname, ifconfig, route), (ssh, ssh-keygen), (firewalld, iptables), (host, nslookup域名信息), (nc/netcat设置路由), ping, (traceroute追踪数据), (netstat端口信息)
#### Linux 硬件管理
- df(查看磁盘空间),du(文件或目录的磁盘空间),top(系统整体运行状态),free(内存),iotop(I/O使用状况)
#### Linux 软件管理
- rpm, yum, apt-get

# linux自动化脚本
- /etc/rc.local(添加脚本)
- /etc/rc.d/init.d(对应启动级别的放置脚本)

# Samba应用
- 安装配置samba
- 用于实现linux和windows的文件共享
- linux真实存在的用户，为其添加smb的密码
- 启动服务，添加防火墙规则

# NTP应用(OSI 应用层)
- 网络时间协议，协议使用udp123

# Firewalld防火墙
- iptables防火墙设置`iptables -A INPUT -p UDP -i eth0 -s 192.168.0.0/24 --dport 123
-j ACCEPT`
- firewalld防火墙设置`firewall-cmd --zone=public --add-port=123
/udp --permanent`

# crontab定时任务
- /etc/crontab文件存放定时任务
```shell
# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed

# 每两个小时以root身份执行 /home/hello.sh 脚本
0 */2 * * * root /home/hello.sh
```

# Systemd应用
- Systemd系统守护进程pid为1
- systemctl(管理系统)
```shell
    # 重启系统
    $ sudo systemctl reboot
    # 关闭系统，切断电源
    $ sudo systemctl poweroff
    # CPU停止工作
    $ sudo systemctl halt
    # 暂停系统
    $ sudo systemctl suspend
    # 让系统进入冬眠状态
    $ sudo systemctl hibernate
    # 让系统进入交互式休眠状态
    $ sudo systemctl hybrid-sleep
    # 启动进入救援状态（单用户状态）
    $ sudo systemctl rescue
```
- Systemd 可以管理所有系统资源。不同的资源统称为12个Unit（单位）。
- Systemd 默认从目录/etc/systemd/system/读取配置文件 启动Unit
- Target 就是一个 Unit 组
- journalctl 统一管理内核和应用日志

# Vim
- 命令模式(命令模式、插入模式、底线命令模式)

# Iptables应用
- 可以检测、修改、转发、重定向和丢弃 IPv4 数据包

# oh-my-zsh 应用

# Nexus 
- Maven 仓库管理器用于搭建私服

# Gitlab
- 自托管的 Git 代码托管平台

# Jenkins
- CI/CD持续集成，持续交付工具

# Svn
- 类似git

# YApi
- API接口管理

# Elastic 技术栈
- ElasticSearch (opens new window)是一个基于 Lucene (opens new window)构建的开源，分布式，RESTful 搜索引擎。
- Logstash (opens new window)传输和处理你的日志、事务或其他数据。
- Kibana(opens new window)将 Elasticsearch 的数据分析并渲染为可视化的报表。
{% asset_img 1774854458843.png Elastic架构 %}
```
以上是 ELK 技术栈的一个架构图。从图中可以清楚的看到数据流向。
Beats(opens new window)是单一用途的数据传输平台，它可以将多台机器的数据发送到 Logstash 或 ElasticSearch。但 Beats 并不是不可或缺的一环，所以本文中暂不介绍。
Logstash(opens new window)是一个动态数据收集管道。支持以 TCP/UDP/HTTP 多种方式收集数据（也可以接受 Beats 传输来的数据），并对数据做进一步丰富或提取字段处理。
ElasticSearch(opens new window)是一个基于 JSON 的分布式的搜索和分析引擎。作为 ELK 的核心，它集中存储数据。
Kibana(opens new window)是 ELK 的用户界面。它将收集的数据进行可视化展示（各种报表、图形化数据），并提供配置、管理 ELK 的界面。
```

# RocketMQ
- MQ消息队列类似kafka

# Nacos
- 发现、配置和管理微服务的软件(注册中心，配置中心)


# 概念介绍
https://www.bilibili.com/video/BV1oCwEeVEe4/?spm_id_from=333.788.videopod.sections&vd_source=4878313de5c2fd6a940f45e90798e004

## Redis 
远程缓存服务(单线程多个sock),加入异步子进程将其持久化到.rdb，传输使用tcp协议。主从模式：主负责读写，从负责读。Keepalived粗糙检测Redis是否存活选择新的主节点。或者使用哨兵节点Sentinel或哨兵集群；哈希槽切分数据；重定向不同的哈希槽信息
## sendfile与mmap[RocketMQ与kafka性能区别]
正常的发送过程：磁盘到内核到用户到内核socket缓冲区到网卡[read, write]；mmap：磁盘到内核到内核socket缓冲区到网卡，用户空间直接映射的内核缓冲区[mmap, write]；sendfile:从磁盘到内核到网卡[ sendfile ]
## Elastic Search
Lucene:{将文本拆分为词项，然后存储包含此词项[ term dictiongry ]的文本索引[ Posting list ]，具体文本保存在[ Stored Fields ]，通过二分查找和树的查找进行优化。Doc Values[ 用于存储特定的索引 ]。segment:[ Inverted Index, Term Index, Stored Fields, Doc Values ]一但生成则不能修改，不定期合并小的segment,读的时候并行的读多个}。根据内容分类设置多个Lucene通过Shard进行区分，内部拆分成多个Shard(独立Lucene库)Shard的优化生成副Shard。涉及到管理的不同节点[ 集群管理， 存储数据,处理请求 ]。使用Raft(一致性算法)进行通信
## Kafka
将q分类成不同的topic,一个topic被拆分成几个Partition，partition被放置到broker上，一个broker中可以有多个partition。为了高可用将partition进行备份，主负责业务，从负责备份。为了持久化将信息保存到磁盘并添加retention policy数据保留策略。为了让消费者从partition中间进行处理对消费者进行分组分别维护自己的消费进度并设置offset定位。使用zookeeper(分布式协调服务)维护集群，后面支持使用Raft
## RocketMQ
将Kafka中的zookeeper替换为Nameserver，RocketMQ的Q只存储摘要信息，具体信息保存到commitLog，修改以Broker为单位进行备份。通过tag标记Q中的消息。原始支持延时队列，原始支持死信队列(重试多次失败)。除了支持offset还支持调整时间
## RabbitMQ:
是一个消息队列，使用不同的优先级，Exchange进行路由，支持集群，Exchange进行同步，Queue进行镜像拷贝(具备主从关系)，基于Erlang进行的数据同步，更靠谱的Raft算法进行选举主Queue
## Docker
只是保存操作系统的用户空间和文件系统以及依赖库。
Docker Compose解决的是一整套服务，Docker Swarm解决的是一整套服务在多个服务器上的部署。VPS(服务器上的虚拟机，不能随时调整资源)ECS是可以动态调整的版本
Docker File根据文件变化来区分不同层的，变化最少的向上放
## HDFS
切分，容错。NameNode[ Namespace文件树,Block Mgr具体文件块 ],DataNode。NameNode的持久化和备份
## Nginx
正向代理(代理浏览器)，反向代理(代理客户端)，网关开放的接口(单线程)而是使用多进程，并且设置一个共享内存，磁盘缓存相应数据。work进行滚动升级使用master进行管理配置
## hadoop
hdfs存储,mapreduce计算,yarn资源管理(将map封装成jvm容器)，Hive查询，spark使用内存进行加速mapreduce，Flink在线的数据处理，高并发hbase分布式数据库
## mysql
存储格式.idb,并将一个表进行分页，通过将每页的第一个主键收集成层级快速查找【并由一个更多策略的buffer pool替代文件缓存】，自适应哈希索引优化热点读性能，change_buffer:辅助索引页先进pool后面再机会的使用磁盘的索引页，Undo_log负责记录写操作，Redo_log_Buffer WAL:对于原子性的操作单独顺序记录到磁盘防止进程崩溃，这些buffer使用Innodb数据引擎管理通过server层提供sql语句支持，server层包括连接管理sql分析规则优化
## mysql的速度优化
使用更大的连接池，增大Buffer Pool
## MongoDB
Document替换列，id替换主键。.wt文件；写时复制[ 单独创建节点写然后替换原来的节点 ]  “没看完”
## PostgreSQL
支持各种各样的索引的扩展[ B-Link(数字文本),GIN(文本|Json),地理位置(GIST),时序数据(BRIN) ]  “没看完”
