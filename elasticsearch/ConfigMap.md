### 创建logstash.conf的ConfigMap

    kubectl create configmap logstash-conf-configmap --from-file=./logstash.conf -n kube-elk
    kubectl get configmap -n kube-elk
    kubectl delete configmap logstash-conf-configmap -n kube-elk