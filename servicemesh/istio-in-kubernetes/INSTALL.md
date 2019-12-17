# 部署和应用

## 安装步骤

```
# 进入文件夹
cd istio-1.4.2

# 添加 istioctl 路径到环境变量 PATH，或将其移动至 /usr/local/bin 下
sudo mv bin/istioctl /usr/local/bin

# 创建命名空间
kubectl create namespace istio-system

# 使用 kubectl apply 安装所有的 Istio CRD
helm template helm/istio-init --name istio-init --namespace istio-system | kubectl apply -f -

# 根据实际情况配置更新 values.yaml
vi install/kubernetes/helm/istio/values.yaml
说明：凡是需要安装的组件，通过修改values.yaml里面对应item进行enable


# 部署 Istio 的核心组件
helm template helm/istio --name istio --namespace istio-system | kubectl apply -f -
```


## 安装结果



```
查看pod运行情况
---
# kubectl get pod -n istio-system

NAME                                      READY   STATUS      RESTARTS   AGE
grafana-7797c87688-x2cdb                  1/1     Running     0          16h
istio-citadel-65c9f49c76-9c5z6            1/1     Running     0          17h
istio-galley-c5cb9c77d-hwkjx              1/1     Running     0          17h
istio-grafana-post-install-1.4.2-2gbmh    0/1     Completed   0          16h
istio-ingressgateway-6d4cb8f7f6-zgrgx     1/1     Running     0          16h
istio-init-crd-10-1.4.2-672g9             0/1     Completed   0          17h
istio-init-crd-11-1.4.2-2dfjl             0/1     Completed   0          17h
istio-init-crd-14-1.4.2-kg7n6             0/1     Completed   0          17h
istio-pilot-568fd746c8-mvlms              2/2     Running     1          17h
istio-policy-79f475c566-h2lgh             2/2     Running     5          17h
istio-security-post-install-1.4.2-xfk42   0/1     Completed   0          17h
istio-sidecar-injector-59ccc94d59-tjfzh   1/1     Running     0          17h
istio-telemetry-6f699b8967-sbbbt          2/2     Running     4          17h
istio-tracing-55c965d5b6-zhv22            1/1     Running     3          16h
kiali-74fdc898b9-njk4j                    1/1     Running     0          16h
prometheus-c8fdbd64f-zjs9w                1/1     Running     0          17h


查看svc
----
# kubectl get svc -n istio-system

NAME                     TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                                                                                                                                      AGE
grafana                  ClusterIP      10.233.38.147   <none>        3000/TCP                                                                                                                                     16h
istio-citadel            ClusterIP      10.233.14.117   <none>        8060/TCP,15014/TCP                                                                                                                           17h
istio-galley             ClusterIP      10.233.8.160    <none>        443/TCP,15014/TCP,9901/TCP                                                                                                                   17h
istio-ingressgateway     LoadBalancer   10.233.23.157   <pending>     15020:32392/TCP,80:31380/TCP,443:31390/TCP,31400:31400/TCP,15029:31949/TCP,15030:31730/TCP,15031:32633/TCP,15032:30889/TCP,15443:31651/TCP   17h
istio-pilot              ClusterIP      10.233.19.79    <none>        15010/TCP,15011/TCP,8080/TCP,15014/TCP                                                                                                       17h
istio-policy             ClusterIP      10.233.57.205   <none>        9091/TCP,15004/TCP,15014/TCP                                                                                                                 17h
istio-sidecar-injector   ClusterIP      10.233.59.114   <none>        443/TCP,15014/TCP                                                                                                                            17h
istio-telemetry          ClusterIP      10.233.57.181   <none>        9091/TCP,15004/TCP,15014/TCP,42422/TCP                                                                                                       17h
jaeger-agent             ClusterIP      None            <none>        5775/UDP,6831/UDP,6832/UDP                                                                                                                   16h
jaeger-collector         ClusterIP      10.233.16.11    <none>        14267/TCP,14268/TCP,14250/TCP                                                                                                                16h
jaeger-query             ClusterIP      10.233.0.152    <none>        16686/TCP                                                                                                                                    16h
kiali                    ClusterIP      10.233.45.73    <none>        20001/TCP                                                                                                                                    16h
prometheus               ClusterIP      10.233.45.158   <none>        9090/TCP                                                                                                                                     17h
tracing                  ClusterIP      10.233.26.69    <none>        80/TCP                                                                                                                                       16h
zipkin                   ClusterIP      10.233.47.177   <none>        9411/TCP

查看暴露的服务ingress
---
# kubectl get ingress -n istio-system

NAME                HOSTS                           ADDRESS   PORTS     AGE
istio-grafana       istio-grafana.xwfintech.com               80, 443   15h
istio-jaeger        istio-jaeger.xwfintech.com                80, 443   15h
istio-kiali         istio-kiali.xwfintech.com                 80, 443   15h
istio-pilot         istio-pilot.xwfintech.com                 80, 443   17h
istio-tracing       istio-tracing.xwfintech.com               80, 443   15h
istio-zipkin        istio-zipkin.xwfintech.com                80, 443   15h
promethus-ingress   istio-promethus.xwfintech.com             80, 443   17h
```


