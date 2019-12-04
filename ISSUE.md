# 关于heapster采集10255端口不通的问题

10255是heapster进行指标采集的端口，在安装的时候kubelet默认设置为0，就是禁用了该端口； 要开启此端口heapster才能进行数据采集

    sed -i 's/--read-only-port=0/--read-only-port=10255/' /etc/kubernetes/kubelet.env
    systemctl restart kubelet
