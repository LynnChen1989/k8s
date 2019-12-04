# 安装和初始化

https://github.com/kubernetes/helm/releases

下载最新版的heml二进制文件，解压安装


在k8s, master节点执行 `heml init` 初始化，会在k8s集群的kube-system的命名空间注入一个 tiller 服务。 作为helm的服务端。 