# Instruction

- Using kube-state-metrics to collect metric of Kubernetes cluster
- Note: We must use RollBinding to allow kube-state-metrics have permission with Kubernetes resource


## Apply all kube-state-metrics manifest 
kubectl apply -f kube-state-metrics/



## Apply prometheus 
kubectl apply -f prometheus-service.yaml
kubectl apply -f configmap-prometheus.yaml
kubectl apply -f prometheus-deployment.yaml



### This is configuration allowed prometheus get metrics


```
- job_name: 'kube-state-metrics'
        static_configs:
          - targets: ['kube-state-metrics.kube-system.svc.cluster.local:8080']
```

==>  http://EXTERNAL-IP:8080 



## Apply grafana to display metrics from prometheus

kubectl apply -f grafana-service.yaml
kubectl apply -f grafana-datasource-config.yaml
kubectl apply -f grafana-deployment.yaml


This is configuration will get metrics from prometheus in /etc/grafana/provisioning/datasources/prometheus.yaml in Grafana container  
 
 
```
"datasources": [
            {
               "access":"proxy",
                "editable": true,
                "name": "prometheus",
                "orgId": 1,
                "type": "prometheus",
                "url": "http://prometheus-service.monitoring.svc:8080",
                "version": 1
            }
        ]
```

==> http://EXTERNAL-IP:3000


