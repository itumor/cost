# Running OpenCost as a Prometheus metric exporter:

```
 helm install my-prometheus --repo https://prometheus-community.github.io/helm-charts prometheus \
  --namespace prometheus --create-namespace \
  --set prometheus-pushgateway.enabled=false \
  --set alertmanager.enabled=false \
  -f extraScrapeConfigs.yaml
```

```

 k delete pod --all  -n prometheus
 k get pod -n prometheus -w
 k logs my-prometheus-server-5cb87d6c94-j8pw8  -n prometheus 
``` 
  my-prometheus-server.prometheus.svc.cluster.local

```
  export POD_NAME=$(kubectl get pods --namespace prometheus -l "app.kubernetes.io/name=prometheus,app.kubernetes.io/instance=my-prometheus" -o jsonpath="{.items[0].metadata.name}")
  kubectl --namespace prometheus port-forward $POD_NAME 9090
```
  http://localhost:9090/targets?search=opencost
```
  helm repo add grafana https://grafana.github.io/helm-charts
  helm repo update

  helm install grafana grafana/grafana \
   --namespace grafana --create-namespace 
```

   grafana.grafana.svc.cluster.local

```
   kubectl get secret --namespace grafana grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```
username :admin
password: ugOYAyzr85Lmhw81MyNjfqMLxt5NYWJYo0v4jMpl
```
     export POD_NAME=$(kubectl get pods --namespace grafana -l "app.kubernetes.io/name=grafana,app.kubernetes.io/instance=grafana" -o jsonpath="{.items[0].metadata.name}")
     kubectl --namespace grafana port-forward $POD_NAME 3000
```
     http://localhost:3000/login

      https://grafana.com/orgs/kubecost


# OpenCost Setup:
```
helm install my-prometheus-opencost --repo https://prometheus-community.github.io/helm-charts prometheus \
  --namespace prometheus-opencost --create-namespace \
  --set prometheus-pushgateway.enabled=false \
  --set alertmanager.enabled=false \
  -f https://raw.githubusercontent.com/opencost/opencost/develop/kubernetes/prometheus/extraScrapeConfigs.yaml
```
  my-prometheus-opencost-server.prometheus-opencost.svc.cluster.local

```
  export POD_NAME=$(kubectl get pods --namespace prometheus-opencost -l "app.kubernetes.io/name=prometheus,app.kubernetes.io/instance=my-prometheus-opencost" -o jsonpath="{.items[0].metadata.name}")
  kubectl --namespace prometheus-opencost port-forward $POD_NAME 9090
```
  kubectl apply --namespace opencost -f https://raw.githubusercontent.com/opencost/opencost/develop/kubernetes/opencost.yaml
```
kubectl port-forward --namespace opencost service/opencost 9004:9003 9009:9090
```

127.0.0.1:9004
127.0.0.1:9009 

# kubecost:
```
helm upgrade --install kubecost \
  --repo https://kubecost.github.io/cost-analyzer/ cost-analyzer \
  --namespace kubecost --create-namespace
k get pod -n kubecost -w

kubectl get secret --namespace kubecost kubecost-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```
# grafana user name and password
username: admin

password: strongpassword
# kubecost URl and grafana
```
kubectl port-forward --namespace kubecost deployment/kubecost-cost-analyzer 9080:9090
```
http://127.0.0.1:9080/overview
http://127.0.0.1:9080/grafana/?orgId=1
