---
title: "Overview"
keywords: 'Kubernetes, KubeSphere, multicluster, hybrid-cloud'
description: 'Overview'

weight: 6110
---

Today, it's very common for organizations to run and manage multiple Kubernetes clusters across different cloud providers or infrastructures. As each Kubernetes cluster is a relatively self-contained unit, the upstream community is struggling to research and develop a multi-cluster management solution. That said, Kubernetes Cluster Federation ([KubeFed](https://github.com/kubernetes-sigs/kubefed) for short) may be a possible approach among others.

The most common use cases of multi-cluster management include service traffic load balancing, development and production isolation, decoupling of data processing and data storage, cross-cloud backup and disaster recovery, flexible allocation of computing resources, low latency access with cross-region services, and vendor lock-in avoidance.

KubeSphere is developed to address multi-cluster and multi-cloud management challenges and implement the proceeding user scenarios, providing users with a unified control plane to distribute applications and its replicas to multiple clusters from public cloud to on-premises environments. KubeSphere also provides rich observability across multiple clusters including centralized monitoring, logging, events, and auditing logs.

![KubeSphere Multi-cluster Management](/images/docs/multi-cluster-overview.jpg)
