# Monitoring

## Introduction

We will setup telegraf to collect metrics of Kubernetes and storage on the influxdb
Assuming we have a influxdb address :  
URL: http://INFLUX-NAME:8086 
DB name: telegraf
User: admin
Password: admin


## Configuration

```
[[inputs.kube_inventory]]

  #this is address of metrics-server--> **kubectl cluster-info**
  
  url = "https://Metrics-server"
  
  bearer_token = "/var/run/secrets/kubernetes.io/serviceaccount/token"
  
  insecure_skip_verify = true

```

## Deploy on Kubernetes

### Define a cluster role using RBAC authorization of Kubernetes  
kubectl apply -f telegraf-clusterRole.yaml

### Create a serviceaccount
kubectl create serviceaccount telegraf

### And then binding above clusterRole for telegraf serviceaccount:
kubectl apply -f telegraf-clusterRoleBinding.yaml

### Create secret 
kubectl apply -f telegraf-secret.yaml

### Create configmap
kubectl apply -f telegraf-configmap.yaml

### Deploy telegraf
kubectl apply -f telegraf-deployment.yaml
