## 背景

由于我的3块数据盘是挂在到k8s集群的3个数据节点的，所以我不通过k8s来创建nfs server，如果使用k8s来创建nfs server，又会存在节点调度的问题。
所以我只需要k8s来创建nfs client provisioner。

#### 宿主机安装nfs

```
yum -y install nfs-utils rpcbind
vim /etc/exports
/data1 *(no_root_squash,rw,sync,no_subtree_check)
exportfs -rv
```

#### 官方模式

    https://github.com/kubernetes/examples/tree/master/staging/volumes/nfs
    pvc(从其他存储) --> nfs-server ---> nfs-client
    
    
#### 本例采用的模式

一个 nfs-client-provisioner 对应一块nfs盘, 所以我最终会创建3个nfs-client-provisioner

##### 操作顺序

    kubectl apply -f nfs-service-account.yml    
    kubectl apply -f nfs-client-deployment.yml
    kubectl apply -f nfs-storage-class.yml 

storageClass创建好了之后，后面就可以动态的分配pvc
