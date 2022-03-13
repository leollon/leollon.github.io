---
title: k8s-learning-day-one
date: 2020-07-17 15:25:23
categories: [Kubernetes]
tags: [Pods,K8s,Kubernetes,Docker]
---

# M$ の K8s Learning Day one

## Pods

- Pods 是在 K8s 里面运行容器的基本单元。
- 一个 Pod 提供设置环境变量，挂载存储以及向一个容器提供其他信息的方式。

Pods 负责运行容器，并且每个 Pod 至少含有一个容器，并且控制容器的执行。容器退出，Pod 也退出。

## ReplicaSets

- ReplicaSets 可以被当作在 K8s 中一个低层次类型的K8s。
- K8s 用户常常选择像 Deployments 以及 DaemonSets这样子的更高层次的抽象。

    一个 ReplicaSet 确保一系列的相同配置的 Pods 按需要的 replica 数量进行运行。如果一个 Pod 退出了，ReplicaSet 运行一个新的 Pod
    来代替退出的旧的那个。

## Secrets

- Secrets 使用 Base64 编码存储，但当关联给一个 Pod 的时候，将会被自动解码。
- Secrets 能够使用文件或环境变量进行关联。
- 使用附加密提供商来加密数据

    Secrets 用来存储非公开的信息，比如 tokens, certificate 或者密码。它能够在运行时关联给 Pods，为的是敏感的配置数据能够安全地
    存储在集群中。

## Deployments

- Deployment 支持滚动更新以及回滚。
- 甚至还可以暂停 Rollouts。

    Deployment 是一个控制部署和维护一个 Pods 集合的更高级的抽象。在这一场景的背后是，它使用一个 ReplicaSet 保持 Pods 处于运行状态中，但是它提供用于在集群中部署，更新以扩展一系列 Pods的复杂逻辑。

## DaemonSets

- DaemonSets 有许多使用模式——一个经常用到模式是使用一个 DaemonSet 在每个主机节点安装或者配置软件。

    DaemonSets 提供一种用于确保一个 Pod 的一个备份运行在集群中的每个节点的方式。随着集群的增长以及缩减，DaemonSet 将这些带有特殊标签的 Pods 分布在所有节点上。

## Ingresses

- 路由流量去往以及来自的集群
- 为多个应用程序提供单个 SSL 端点（endpoint）
- 一个 ingress 的多个不同实现允许为某些的平台客制化。

    Ingresses 提供一种用于声明流量应该从集群外部被导向集群内的目的节点的方式。单个外部 Ingress 端点（point）可以接收发往多个
    不同内部服务的流量。

## CronJobs

- 使用常见的 Cron 语法来调度任务
- CronJobs 是用于创建短期服务器工具的 Batch API 的一部分。

    CronJobs 提供一种用于调度 Pods 执行的方法。对于运行比如备份，报告以及自动化测试等周期性任务来说，是很优秀了。

## CRDs

- 一个 CRD 定义了一个新的资源类型，并且告知 Kubernetes。
- 一旦增加一个新的资源类型，则可能创建那个资源类型的新实例。
- 处理 CRD 变化由你决定。一个常见的模式是创建一个自定义的控制器，控制器监控新的 CRD 实例并且根据情况来响应。

    CustomResourceDifinitions，或者 CRDs，提供一种扩展机制，集群运维人员以及开发人员能够使用这一机制来创建他们自己的资源类型。
