---
title: "KubeSphere 应用商店"
keywords: "Kubernetes, KubeSphere, app-store, OpenPitrix"
description: "如何启用 KubeSphere 应用商店"

linkTitle: "KubeSphere 应用商店"
weight: 3515
---

## 什么是 KubeSphere 应用商店

作为一个开源的、以应用为中心的容器平台，KubeSphere 在 [OpenPitrix](https://github.com/openpitrix/openpitrix) 的基础上，为用户提供了一个基于 Helm 的应用商店，用于应用生命周期管理。OpenPitrix 是一个开源的 Web 平台，用于打包、部署和管理不同类型的应用。KubeSphere 应用商店允许 ISV、开发者和用户在一站式服务中只需点击几下就可以上传、测试、部署和发布应用。

对内，KubeSphere 应用商店可以作为不同团队共享数据、中间件和办公应用的场所。对外，有利于制定行业标准的建设和交付。默认情况下，应用商店中内置了 15 个应用。启用该功能后，可以通过应用模板添加更多应用。

![应用商店](https://ap3.qingstor.com/kubesphere-website/docs/20200828170503.png)

有关更多信息，请参阅[应用商店](../../application-store/)。

## 在安装前启用应用商店

### 在 Linux 上安装

当您在 Linux 上安装多节点 KubeSphere 时，首先需要创建一个配置文件，该文件列出了所有 KubeSphere 组件。

1. 基于在 [Linux 上安装 KubeSphere](../../installing-on-linux/introduction/multioverview/) 的教程，创建了一个默认文件 **config-sample.yaml**，通过执行以下命令修改该文件：

    ```bash
    vi config-sample.yaml
    ```

    {{< notice note >}}
如果采用 [All-in-one 安装](../../quick-start/all-in-one-on-linux/)，则不需要创建 `config-sample.yaml` 文件，因为可以直接创建集群。一般来说，All-in-one 模式是为那些刚刚接触 KubeSphere 并希望熟悉系统的用户准备的，如果您想在这个模式下启用应用商店（比如出于测试的目的），可以参考[下面的部分](#在安装后启用应用商店)安装后如何启用应用商店。
    {{</ notice >}}

2. 在该文件中，搜寻到`openpitrix`，并将`enabled`的`false`改为`true`，完成后保存文件。

    ```bash
    openpitrix:
        enabled: true # Change "false" to "true"
    ```

3. 使用配置文件创建集群：

    ```bash
    ./kk create cluster -f config-sample.yaml
    ```

### 在 Kubernetes 上安装

在已有 Kubernetes 集群上安装 KubeSphere 时，需要部署 [ks-installer](https://github.com/kubesphere/ks-installer/) 的两个 yaml 文件，如下面所示。

1. 首先下载 [cluster-configuration.yaml](https://github.com/kubesphere/ks-installer/releases/download/v3.0.0/cluster-configuration.yaml) 文件，然后打开并开始编辑。

    ```bash
    vi cluster-configuration.yaml
    ```

2. 在这个本地 `cluster-configuration.yaml` 文件中，搜寻到 `openpitrix`，并将  `enabled` 的 `false` 改为 `true`，完成后保存文件。

    ```bash
    openpitrix:
        enabled: true # Change "false" to "true"
    ```

3. 执行以下命令开始安装：

    ```bash
    kubectl apply -f https://github.com/kubesphere/ks-installer/releases/download/v3.0.0/kubesphere-installer.yaml

    kubectl apply -f cluster-configuration.yaml
    ```

## 在安装后启用应用商店

1. 以`admin`身份登录控制台，点击左上角的**平台管理**，选择**集群管理**。

    ![集群管理](https://ap3.qingstor.com/kubesphere-website/docs/20200828111130.png)

2. 点击 **自定义资源 CRD**，在搜索栏中输入`clusterconfiguration`，点击结果查看其详细页面。

    {{< notice info >}}
自定义资源定义（CRD）允许用户在不增加额外的 API 服务器的情况下创建一种新的资源类型，用户可以像使用其它 Kubernetes 内置对象一样使用这些自定义资源。
    {{</ notice >}}

3. 在**资源列表**中，点击`ks-installer`右边的三个点，选择**编辑 YAML**。

    ![编辑 YAML](https://ap3.qingstor.com/kubesphere-website/docs/20200827182002.png)

4. 在这个 YAML 文件中，搜寻到`openpitrix`，将`enabled`的`false`改为`true`。完成后，点击右下角的**更新**，保存配置。

    ```bash
    openpitrix:
        enabled: true # Change "false" to "true"
    ```

5. 您可以通过执行以下命令，使用 Web Kubectl 工具来检查安装过程：

    ```bash
    kubectl logs -n kubesphere-system $(kubectl get pod -n kubesphere-system -l app=ks-install -o jsonpath='{.items[0].metadata.name}') -f
    ```

    {{< notice tip >}}
您可以通过点击控制台右下角的锤子图标找到 Kubectl 工具。
    {{</ notice >}}

## 验证组件的安装

{{< tabs >}}

{{< tab "在仪表板中验证组件的安装" >}}

进入**服务组件**，检查 **OpenPitrix** 的状态，可以看到如下类似图片：

![openpitrix](https://ap3.qingstor.com/kubesphere-website/docs/20200829124018.png)

{{</ tab >}}

{{< tab "通过 kubectl 验证组件的安装" >}}

执行以下命令来检查 Pod 的状态：

```bash
kubectl get pod -n openpitrix-system
```

如果组件运行成功，输出结果如下：

```bash
NAME                                                READY   STATUS      RESTARTS   AGE
hyperpitrix-generate-kubeconfig-pznht               0/2     Completed   0          1h6m
hyperpitrix-release-app-job-hzdjf                   0/1     Completed   0          1h6m
openpitrix-hyperpitrix-deployment-fb76645f4-crvmm   1/1     Running     0          1h6m
```

{{</ tab >}}

{{</ tabs >}}
