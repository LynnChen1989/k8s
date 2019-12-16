
cd /root/k8s/servicemesh/istio-in-kubernetes

helm template helm/istio-init --name istio-init --namespace istio-system | kubectl apply -f -
helm template helm/istio --name istio --namespace istio-system | kubectl apply -f -