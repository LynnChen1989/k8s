# 对象分类

类别  | 名称 
---|---
资源对象 | Pod、ReplicaSet、ReplicationController、Deployment、StatefulSet、DaemonSet、Job、CronJob、HorizontalPodAutoscaling
配置对象 | Node、Namespace、Service、Secret、ConfigMap、Ingress、Label、ThirdPartyResource、 ServiceAccount
存储对象 | Volume、Persistent Volume
策略对象 	| SecurityContext、ResourceQuota、LimitRange


# 对象描述

#### 必须字段

- apiVersion - 创建该对象所使用的 Kubernetes API 的版本
- kind - 想要创建的对象的类型
- metadata - 帮助识别对象唯一性的数据，包括一个 name 字符串、UID 和可选的 namespace


# Pod相关
在Kubrenetes集群中Pod有如下两种使用方式：

- 一个Pod中运行一个容器。“每个Pod中一个容器”的模式是`最常见`的用法；在这种使用方式中，你可以把Pod想象成是单个容器的封装，kuberentes管理的是Pod而不是直接管理容器。
 
- 在一个Pod中同时运行多个容器。一个Pod中也可以同时封装几个需要紧密耦合互相协作的容器，它们之间共享资源。这些在同一个Pod中的容器可以互相协作成为一个service单位——一个容器共享文件，另一个“sidecar”容器来更新这些文件。Pod将这些容器的存储资源作为一个实体来管理。


每个Pod都是应用的一个实例。如果你想平行扩展应用的话（运行多个实例），你应该运行多个Pod，每个Pod都是一个应用实例。在Kubernetes中，这通常被称为replication。

Pod中可以共享两种资源：网络和存储。

# 集群资源

禁止pod调度到该节点上

    kubectl cordon <node>

驱逐该节点上的所有pod

    kubectl drain <node>

该命令会删除该节点上的所有Pod（DaemonSet除外），在其他node上重新启动它们，通常该节点需要维护时使用该命令。直接使用该命令会自动调用`kubectl cordon <node>`命令。当该节点维护完成，启动了kubelet后，再使用`kubectl uncordon <node>`即可将该节点添加到kubernetes集群中。


#### 标签选择器

Label selector有两种类型：

- equality-based ：可以使用=、==、!=操作符，可以使用逗号分隔多个表达式
- set-based ：可以使用in、notin、!操作符，另外还可以没有操作符，直接写出某个label的key，表示过滤有某个key的object而不管该key的value是何值，!表示没有该label的object

#### Taint,Toleration


# 控制器

### Deployment

典型的应用场景包括：

    定义Deployment来创建Pod和ReplicaSet
    滚动升级和回滚应用
    扩容和缩容
    暂停和继续Deployment



##### 扩容：

    kubectl scale deployment nginx-deployment --replicas 10

##### 如果集群支持 horizontal pod autoscaling 的话，还可以为Deployment设置自动扩展：

    kubectl autoscale deployment nginx-deployment --min=10 --max=15 --cpu-percent=80

#####  更新镜像也比较简单:

    kubectl set image deployment/nginx-deployment nginx=nginx:1.9.1

##### 回滚：

    kubectl rollout undo deployment/nginx-deployment


# secret

创建好secret之后，有两种方式来使用它：

- 以Volume方式
- 以环境变量方式


# 工具

- kubectx：用于切换kubernetes context
- kube-ps1：为命令行终端增加$PROMPT字段
- kube-shell：交互式带命令提示的kubectl终端

# 认证

您可以一次性启用多种身份验证方式。通常使用至少以下两种认证方式：

- 服务帐户的 service account token
- 至少一种其他的用户认证的方式




