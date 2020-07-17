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
