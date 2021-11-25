# Deploy cassandra using DataStax Kubernetes Operator

Quickly spin up a local kubernetes cluster and deploy Cassandra using cass-operator

## Prerequisites
- Docker
- Minikube


### 1. Start the Kubernetes cluster
```bash
minikube start --cpus=5 --memory='10128m' --kubernetes-version=1.21.2
```


