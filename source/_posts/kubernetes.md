---
title: kubernetes
date: 2026-03-29T22:49:39+08:00
tags: 快速入门
---
资源对象：
node(内部ip地址)

- pod(最小调度单元，对容器的封装)
- service(将一组pod封装成服务，可以跨node)
- ingress(集群外部访问集群内部服务,可配置域名，负载均衡，ssl证书)
- configmap(明文，配置信息和镜像内容分离)
- secret(敏感信息)
- volumes(持久化资源挂载到外部)
- deploy(管理pod的副本数量，和更新)
- statefulSet(deploy管理不了有状态的pod，增加持久化保证)

架构：
1. Work_Node包含的组件：
- kubelet(管理节点的pod)
- kube-proxy(网络和负载均衡，代理)
- container-runtime(具体运行时)
  - containerd/Docker-Engine/CRI-O
    Master_Node
2. Control_plane包含的组件：
- API server(通信所有组件，kubectl连接它，集群资源对象的认证访问控制)
- sched(调度器)
- Controller Manager(监控各种资源状态)
- etcd(键值存储系统)
