# 术语表 {#glossary}

云容器引擎提供Kubernetes原生API，支持使用kubectl，且提供图形化控制台KubeSphere，让您能够拥有完整的端到端使用体验，使用容器化平台前，建议您先了解相关的基本概念。
[[TOC]]

## 集群 (Cluster) {#Cluster}

集群指容器运行所需要的云资源组合，关联了若干云服务器节点、负载均衡等云资源。您可以理解为集群是“同一个子网中一个或多个弹性云服务器（又称：节点）”通过相关技术组合而成的计算机群体，为容器运行提供了计算资源池。


## 节点 (Node) {#Node}
每一个节点对应一台服务器（可以是虚拟机实例或者物理服务器），容器应用运行在节点上。节点上运行着Agent代理程序（kubelet），用于管理节点上运行的容器实例。集群中的节点数量可以伸缩。

## 节点池（NodePool）{#NodePool}
节点池是集群中具有相同配置的一组节点，一个节点池包含一个节点或多个节点。

## 虚拟私有云（VPC）{#VPC}
虚拟私有云是通过逻辑方式进行网络隔离，提供安全、隔离的网络环境。您可以在VPC中定义与传统网络无差别的虚拟网络，同时提供弹性IP、安全组等高级网络服务。

## 实例（Pod） {#pod}
实例（Pod）是 Kubernetes 部署应用或服务的最小的基本单位。一个Pod 封装多个应用容器（也可以只有一个容器）、存储资源、一个独立的网络 IP 以及管理控制容器运行方式的策略选项。

图2 实例（Pod）
![](https://caapap-images.oss-cn-hangzhou.aliyuncs.com/img/k8szh-cn_image_0199190051.png)

## 容器（Container） {#container}
一个通过 Docker 镜像创建的运行实例，一个节点可运行多个容器。容器的实质是进程，但与直接在宿主执行的进程不同，容器进程运行于属于自己的独立的命名空间。

图3 实例Pod、容器Container、节点Node的关系
![](https://caapap-images.oss-cn-hangzhou.aliyuncs.com/img/k8szh-cn_image_0000001096321618.png)
## 工作负载 (Workload) {#workload}
工作负载是在Kubernetes上运行的应用程序。无论您的工作负载是单个组件还是协同工作的多个组件，您都可以在Kubernetes上的一组Pod中运行它。在Kubernetes中，工作负载是对一组Pod的抽象模型，用于描述业务的运行载体，包括Deployment、StatefulSet、DaemonSet、Job、CronJob等多种类型。
- 无状态工作负载：即Kubernetes中的“Deployment”，无状态工作负载支持弹性伸缩与滚动升级，适用于实例完全独立、功能相同的场景，如：nginx、wordpress等。
- 有状态工作负载：即Kubernetes中的“StatefulSet”，有状态工作负载支持实例有序部署和删除，支持持久化存储，适用于实例间存在互访的场景，如ETCD、mysql-HA等。
- 创建守护进程集：即Kubernetes中的“DaemonSet”，守护进程集确保全部（或者某些）节点都运行一个Pod实例，支持实例动态添加到新节点，适用于实例在每个节点上都需要运行的场景，如ceph、fluentd、Prometheus Node Exporter等。
- 普通任务：即Kubernetes中的“Job”，普通任务是一次性运行的短任务，部署完成后即可执行。使用场景为在创建工作负载前，执行普通任务，将镜像上传至镜像仓库。
- 定时任务：即Kubernetes中的“CronJob”，定时任务是按照指定时间周期运行的短任务。使用场景为在某个固定时间点，为所有运行中的节点做时间同步。
图4 工作负载与Pod的关系
![](https://caapap-images.oss-cn-hangzhou.aliyuncs.com/img/k8szh-cn_image_0199190052.png)

## 镜像 (Image) {#image}
Docker镜像是一个模板，是容器应用打包的标准格式，用于创建Docker容器。或者说，Docker镜像是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的配置参数（如匿名卷、环境变量、用户等）。镜像不包含任何动态数据，其内容在构建之后也不会被改变。在部署容器化应用时可以指定镜像，镜像可以来自于 Docker Hub、容器镜像服务或者用户的私有 Registry。例如一个Docker镜像可以包含一个完整的Ubuntu操作系统环境，里面仅安装了用户需要的应用程序及其依赖文件。

镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。

图5 镜像、容器、工作负载的关系
![](https://caapap-images.oss-cn-hangzhou.aliyuncs.com/img/k8szh-cn_image_0000001095998576.png)

## 命名空间 (Namespace) {#namespace}
- 命名空间是对一组资源和对象的抽象整合。在同一个集群内可创建不同的命名空间，不同命名空间中的数据彼此隔离。使得它们既可以共享同一个集群的服务，也能够互不干扰。例如：

- 可以将开发环境、测试环境的业务分别放在不同的命名空间。
常见的pods，services，replication controllers和deployments等都是属于某一个namespace的（默认是default），而node，persistentVolumes等则不属于任何namespace。

## 服务 (Service) {#service}
服务是一组逻辑上的Pod的抽象，通常用来访问一个应用。服务可以通过标签选择器找到对应的Pod，从而实现对Pod的访问。服务可以通过ClusterIP、NodePort、LoadBalancer、ExternalName等方式对外提供访问入口。

Kubernetes允许指定一个需要的类型的Service，类型 的取值以及行为如下：
- ClusterIP：默认类型，该类型的Service只能在集群内部访问，通过该类型的Service暴露的Pod只能被集群内部的其他应用访问，集群外部的请求将无法访问到该Service。
- NodePort：该类型的Service将会在每个Node上绑定一个端口，通过该类型的Service暴露的Pod可以通过NodeIP:NodePort的方式访问到，集群外部的请求也可以通过NodeIP:NodePort的方式访问到该Service。
- LoadBalancer：该类型的Service将会在云厂商提供的负载均衡器上绑定一个端口，通过该类型的Service暴露的Pod可以通过负载均衡器的IP:Port的方式访问到，集群外部的请求也可以通过负载均衡器的IP:Port的方式访问到该Service。
- ExternalName：该类型的Service将会在集群内部创建一个CNAME记录，通过该类型的Service暴露的Pod可以通过CNAME的方式访问到，集群外部的请求也可以通过CNAME的方式访问到该Service。

## 七层负载均衡 (Ingress) {#ingress}
Ingress是对集群外部的访问提供了统一的入口，Ingress可以提供负载均衡、SSL终结、基于名称的虚拟主机等功能。Ingress并不会直接将请求转发给后端的Pod，而是通过Controller将请求转发给后端的Pod。

## 网络策略 (NetworkPolicy) {#networkpolicy}
网络策略是一组规则，用于控制集群内部Pod的网络访问。网络策略可以控制Pod的入站和出站流量，可以控制Pod之间的流量，也可以控制Pod与外部网络的流量。

## 配置项 (ConfigMap) {#configmap}
配置项是用来存储配置数据的对象，配置项可以存储数据库连接字符串、密钥、命令行参数等配置数据。配置项可以通过环境变量、命令行参数、配置卷等方式注入到Pod中。

## 密钥 (Secret) {#secret}
密钥是用来存储敏感数据的对象，密钥可以存储密码、OAuth令牌、SSH密钥等敏感数据。密钥可以通过环境变量、命令行参数、配置卷等方式注入到Pod中。

## 标签 (Label) {#label}
标签是用来标识Kubernetes对象的键值对，标签可以用来标识对象的用途、所有者、环境等信息。标签可以通过标签选择器来选择对象。

## 选择器 (Selector) {#selector}
Label selector是Kubernetes核心的分组机制，通过label selector客户端/用户能够识别一组有共同特征或属性的资源对象。

## 注解 (Annotation) {#annotation}
注解是用来存储非标识性的辅助数据的对象，注解可以存储构建信息、镜像信息、资源配额信息等辅助数据。注解不可以通过标签选择器来选择对象。

## 持久化存储 (PersistentVolume) {#persistentvolume}
PersistentVolume是一种存储资源，用于提供持久化存储。PersistentVolume可以通过手动创建、动态创建、静态创建等方式创建。

## 持久化存储声明 (PersistentVolumeClaim) {#persistentvolumeclaim}
PV 是存储资源，而 PersistentVolumeClaim (PVC) 是对 PV 的请求。PVC 跟 Pod 类似：Pod 消费 Node 资源，而 PVC 消费 PV 资源；Pod 能够请求 CPU 和内存资源，而 PVC 请求特定大小和访问模式的数据卷。

## 弹性伸缩 (HPA) {#hpa}
弹性伸缩是一种自动调整应用实例数量的机制，弹性伸缩可以根据CPU使用率、内存使用率等指标自动调整应用实例数量。

## 亲和性与反亲和性 (Affinity and Anti-Affinity) {#affinity-and-anti-affinity}
在应用没有容器化之前，原先一个虚机上会装多个组件，进程间会有通信。但在做容器化拆分的时候，往往直接按进程拆分容器，比如业务进程一个容器，监控日志处理或者本地数据放在另一个容器，并且有独立的生命周期。这时如果分布在网络中两个较远的点，请求经过多次转发，性能会很差。

- 亲和性：可以实现就近部署，增强网络能力实现通信上的就近路由，减少网络的损耗。如：应用A与应用B两个应用频繁交互，所以有必要利用亲和性让两个应用的尽可能的靠近，甚至在一个节点上，以减少因网络通信而带来的性能损耗。
- 反亲和性：主要是出于高可靠性考虑，尽量分散实例，某个节点故障的时候，对应用的影响只是 N 分之一或者只是一个实例。如：当应用采用多副本部署时，有必要采用反亲和性让各个应用实例打散分布在各个节点上，以提高HA。

## 节点亲和性（NodeAffinity） {#nodeaffinity}
通过选择标签的方式，可以限制pod被调度到特定的节点上。

## 节点反亲和性（NodeAntiAffinity） {#nodeantiaffinity}
通过选择标签的方式，可以限制pod不被调度到特定的节点上。

## 工作负载亲和性（PodAffinity） {#podaffinity}
指定工作负载部署在相同节点。用户可根据业务需求进行工作负载的就近部署，容器间通信就近路由，减少网络消耗。

## 工作负载反亲和性（PodAntiAffinity） {#podantiaffinity}
指定工作负载部署在不同节点。同个工作负载的多个实例反亲和部署，减少宕机影响；互相干扰的应用反亲和部署，避免干扰。

## 资源配额（Resource Quota） {#resourcequota}
资源配额（Resource Quotas）是用来限制用户资源用量的一种机制。

## 资源限制（Limit Range） {#limitrange}
默认情况下，K8s中所有容器都没有任何CPU和内存限制。LimitRange(简称limits)用来给Namespace增加一个资源限制，包括最小、最大和默认资源。在pod创建时，强制执行使用limits的参数分配资源。

## 环境变量 (Environment Variable) {#environment-variable}
环境变量是指容器运行环境中设定的一个变量，您可以在创建容器模板时设定不超过30个的环境变量。环境变量可以在工作负载部署后修改，为工作负载提供了极大的灵活性。


## 模板（Chart） {#chart}
Kubernetes集群可以通过Helm实现软件包管理，这里的Kubernetes软件包被称为模板（Chart）。Helm对于Kubernetes的关系类似于在Ubuntu系统中使用的apt命令，或是在CentOS系统中使用的yum命令，它能够快速查找、下载和安装模板（Chart）。

模板（Chart）是一种Helm的打包格式，它只是描述了一组相关的集群资源定义，而不是真正的容器镜像包。模板中仅仅包含了用于部署Kubernetes应用的一系列YAML文件，您可以在Helm模板中自定义应用程序的一些参数设置。在模板的实际安装过程中，Helm会根据模板中的YAML文件定义在集群中部署资源，相关的容器镜像并不会包含在模板包中，而是依旧从YAML中定义好的镜像仓库中进行拉取。

对于应用开发者而言，需要将容器镜像包发布到镜像仓库，并通过Helm的模板将安装应用时的依赖关系统一打包，预置一些关键参数，来降低应用的部署难度。

对于应用使用者而言，可以使用Helm查找模板（Chart）包并支持调整自定义参数。Helm会根据模板包中的YAML文件直接在集群中安装应用程序及其依赖，应用使用者不用编写复杂的应用部署文件，即可以实现简单的应用查找、安装、升级、回滚、卸载。

