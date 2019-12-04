# HPA(Horizontal Pod Autoscaing)


    kubectl get hpa -n kube-devops
    kubectl describe hpa hpa-test-app-hpa -n kube-devops  # 查看内容进行排查
    
    kubectl exec -it busybox /bin/sh
    / # while true; do wget -q -O- 10.254.155.132:8080; done
注意：1.10部署有问题，1.8.x可以
