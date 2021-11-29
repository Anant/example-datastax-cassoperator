# Deploy cassandra using DataStax Kubernetes Operator

Quickly spin up a local kubernetes cluster and deploy Cassandra using cass-operator

## Prerequisites
- Docker
- Minikube


## 1. Start the Kubernetes cluster
```bash
minikube start --cpus=5 --memory='10128m' --kubernetes-version=1.22.4
```
## 2. Install the cert-manager
```bash
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.5.3/cert-manager.yaml
```
## 3. Set up the StorageClass
```bash
kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml
```
### 3.1 Change the default StorageClass
```bash
kubectl patch storageclass standard -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"false"}}}'
```
```bash
kubectl patch storageclass local-path -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```
## 4. Default installation in cass-operator namespace
```bash
kubectl apply -k github.com/k8ssandra/cass-operator/config/deployments/default
```
## 5. Cassandra Datacenter
```bash
kubectl -n cass-operator apply -f dc.yaml
```
### 5.1 Check status
```bash
kubectl -n cass-operator get cassdc/dc1 -o "jsonpath={.status.cassandraOperatorProgress}"
```
```bash
kubectl -n cass-operator exec -it -c cassandra cluster1-dc1-default-sts-0 -- nodetool status
```
### 5.2 Auth
```bash
CASS_USER=$(kubectl -n cass-operator get secret cluster1-superuser -o json | jq -r '.data.username' | base64 --decode)
```
```bash
CASS_PASS=$(kubectl -n cass-operator get secret cluster1-superuser -o json | jq -r '.data.password' | base64 --decode)
```
```bash
kubectl -n cass-operator exec -ti cluster1-dc1-default-sts-0 -c cassandra -- sh -c "cqlsh -u '$CASS_USER' -p '$CASS_PASS'"
```

