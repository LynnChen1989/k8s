### 部署方式

采用hostNetwork的方式

```
# 给ingress节点打上标签，入口流量都进入到这台服务器
 kubectl label nodes xw-ops-k8s-node06 ingress-controller=true

```
### 查看配置文件

```
kubectl exec  nginx-ingress-controller-b854df877-hsch7 -n kube-system -- cat /etc/nginx/nginx.conf
```