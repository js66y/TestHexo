---
title: redis
date: 2026-04-11T20:06:14+08:00
tags:
---

接下来Redis很有用...
https://www.bilibili.com/video/BV1cr4y1671t?spm_id_from=333.788.videopod.episodes&vd_source=4878313de5c2fd6a940f45e90798e004&p=24
<!-- more -->
通用命令：
- KEYS:查看符合模板的所有key，不建议在生产环境设备上使用
- DEL:删除一个指定的key
- EXISTS:判断key是否存在
- EXPIRE:给一个key设置有效期，有效期到期时该key会被自动删除
- TTL:查看一个KEY的剩余有效期

Key的层级格式
- `[项目名]:[业务名]:[类型]:[id]`

Hash类型:在key下面存储了key和value


工程实践:
1. 使用在session中
- Session:存储在服务器内存，作为用户状态的上下文，cookie中记录session_id得到对应用户包括登录的状态。
- 在多tomcat的时候session不可共享，这时候使用redis
2. 使用在缓存中
- 不同的淘汰机制[内存淘汰，过期淘汰，主动更新]
- 主动更新方法[Cache Aside,Read/Write Through,Write Back]
- Cache Aside模式问题[更新还是删除，先数据库还是先缓存，原子性]
- 查询最佳实践
    1. 先查询缓存
    2. 如果缓存命中，直接返回
    3. 如果缓存未命中，则查询数据库
    4. 将数据库数据写入缓存
    5. 返回结果
- 修改最佳实践
    1. 先修改数据库
    2. 然后删除缓存
    3. 确定原子性
- 缓存穿透问题[不存在的数据缓存空，布隆过滤]
- 缓存雪崩问题[大量过期，随机ttl]
- 缓存击穿问题[热点key不过期，重建加锁]
3. 分布式锁
- 互斥性，可用性，高性能，安全性
- 获取锁`set lock trread1 nx ex 10`
- 释放锁`del key`