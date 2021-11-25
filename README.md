# Deploy cassandra using DataStax Kubernetes Operator

Quickly spin up a local kubernetes cluster and deploy Cassandra using cass-operator

## Prerequisites
- Docker
- Minikube


### 1. Start the Kubernetes cluster
```bash
minikube start --cpus=5 --memory='10128m' --kubernetes-version=1.21.2
```
### 2. Set up the StorageClass
```bash
kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml
```
## 2.1 Change the default StorageClass
```bash
kubectl patch storageclass standard -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"false"}}}'
```


