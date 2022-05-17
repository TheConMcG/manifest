## Clone repo & cd into it, then run:

```
minikube start --driver=docker
kubectl create namespace monitoring
kubectl apply -f Manifest/
```

***

### Or run whichever deployments you don't already have: 


  

Prometheus Deployment:

```
kubectl create -f prometheus-clusterRole.yaml
kubectl create -f prometheus-config-map.yaml
kubectl create -f prometheus-deployment.yaml
kubectl create -f prometheus-service.yaml --namespace=monitoring
kubectl get deployments --namespace=monitoring
```

Kube State Metrics:

```
kubectl create -f ksm-cluster-role.yaml
kubectl create -f ksm-cluster-role-binding.yaml
kubectl create -f ksm-deployment.yaml
kubectl create -f ksm-service-account.yaml
kubectl create -f ksm-service.yaml
```

Grafana Deployment:

```
kubectl create -f grafana-datasource-config.yaml
kubectl create -f grafana-deployment.yaml
kubectl create -f grafana-service.yaml
```

Node Exporter:

```
kubectl create -f node-exporter-daemonset.yaml
kubectl create -f node-exporter-service.yaml
```

Kompass:

```
kubectl create -f kompass-depl.yaml
```

***

  

### Display your grafana & prometheus pod names:

```
kubectl get pod --namespace=monitoring
```

  

***

### Forward ports to view application:
Open a new tab in your console with command+'t' and forward your grafana port:
```
kubectl port-forward <grafana-pod-name> --namespace=<namespace-grafana-is-in> 3000:3000
```

Open a new tab in your console with command+'t' and forward your prometheus port:
```
kubectl port-forward <prometheus-pod-name> --namespace=<namespace-prometheus-is-in> 9090:9090
```

***

### Grafana Example:

```
kubectl port-forward grafana-5f9587668c-9tkjw --namespace=monitoring 3000:3000
```

Then navigate to localhost:3000 to view grafana (username and password should both be 'admin'), if you hover over the '+' icon, you should see the option to 'import'. Click on import and enter 3119 as the dashboard to import, this should give you a variety of charts, some of which are working with this current set up.

  

***

### Prometheus Example:

```
kubectl port-forward prometheus-deployment-84f65c89c5-lmxl4 --namespace=monitoring 9090:9090
```

Then navigate to localhost:9090 to view prometheus. You can try out your own queries such as 'kubelet_http_requests_total' and view the table or graph of your metrics.