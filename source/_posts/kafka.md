---
title: cm
date: 2026-04-11T12:36:31+08:00
tags:
---

接下来kafka很有用...
<!-- more -->
kafka基于hadoop，通过bin里面的sh命令进行使用，需要jvm环境
消费者组：
- 同一个消费者组只有一个消费者实际消费生产的消息一个组共用offset
- Topic不同主题
- Patition分成部分
- Lag还有多少没有消费
- Offset消费到哪里了

服务端管理Offset,客户端修改Offset，但是这可能的存在混乱（服务端挂了，Offset不一致等等）。【客户端自己管理】

拦截者拦截器（继承ProducerInterceptor实现生产者提交过程中的修改或者拦截）

消息的传递，序列化的方法是用户自定义的通过继承Serializer进行实现

生产者的消息发到哪个Patition【策略】
- 默认发往同一个分区，不超过额值时
    - 如果有key,则根据key的哈希进行
    - 如果没有key,则跟随前面的消息
- 轮转的发送
- 自己实现Partitioner

{% asset_img 1775906426127.png 生产者提交的过程，这当中会出现很多考点1 %}

客户端ack设置:
- 0只要保证发送成果
- 1只要保证leader成功写入成功
- all所有leader写入成功
服务端ack设置:
- 在客户端设置all/-1时候，设置几个成功后返回ack,默认1

生产者消息幂等性
- at-least-once,at-most-once,Exactly-once(通过添加SN:[PID、Partiton]，设置开启有条件包括ack设置为1)

消息也可以作为事务进行发送
{% asset_img 1775907958085.png 生产者提交的过程，这当中会出现很多考点2 %}