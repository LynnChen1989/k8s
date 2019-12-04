## 创建grafana.ini的configmap
    
    kubectl create configmap grafana-ini-configmap --from-file=./grafana.ini -n kube-system