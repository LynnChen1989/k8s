## 集群管理命令

#### 获取集群角色相关命令

    kubectl get clusterrolebinding 
    kubectl get clusterrole
    


## 关于service account/ cluster role

查看token

    kubectl describe serviceaccount/traefik-ingress-controller -n kube-snake

或
    kubectl get serviceaccount/traefik-ingress-controller -n kube-snake -o yaml

输出


```
apiVersion: v1
kind: ServiceAccount
metadata:
  creationTimestamp: 2018-06-08T08:43:58Z
  name: traefik-ingress-controller
  namespace: kube-snake
  resourceVersion: "18701803"
  selfLink: /api/v1/namespaces/kube-snake/serviceaccounts/traefik-ingress-controller
  uid: 1596b49b-6af8-11e8-a850-52540084fb0a
secrets:
- name: traefik-ingress-controller-token-bngt9
```

然后您将看到有一个 token 已经被自动创建，并被 service account 引用



## 获取cluster domain


任意进入一个pod查看即可

```
(.venv) [root@k8s-master-0001 traefix]# kubectl exec -it curl bash -n kube-snake
root@curl:/# cat /etc/resolv.conf
nameserver 10.233.0.3
search kube-snake.svc.cluster.local svc.cluster.local cluster.local
options ndots:5
```

## 使用https访问k8s内的服务

### 1.创建secret保存https证书

证书使用是之前搭建kubernetes集群使用的证书

```
   kubectl create secret generic traefik-cert --from-file=ops.dragonest.com.cert --from-file=ops.dragonest.com.key -n kube-devops
   kubectl delete secret traefik-cert -n kube-devops
```

### 2.创建configmap保存traefik配置文件

traefik.toml内容

```
defaultEntryPoints = ["http","https"]

[entryPoints]
  [entryPoints.http]
  address = ":80"
    [entryPoints.http.redirect]
      entryPoint = "https"

  [entryPoints.https]
  address = ":443"
    [entryPoints.https.tls]
      [[entryPoints.https.tls.certificates]]
      CertFile = "/ssl/_.dragonest.com.pem"
      KeyFile = "/ssl/_.dragonest.com.key"
#      [[entryPoints.https.tls.certificates]]
#      CertFile = "integration/fixtures/https/snitest.org.cert"
#      KeyFile = "integration/fixtures/https/snitest.org.key"

```

### 3.创建`traefik.toml`的configmap

    kubectl create configmap traefik-conf  --from-file=traefik.toml -n kube-system
    
    
### 4.执行 `traefix-deployment.yaml`