# Inter VN ping
## Create default project
```
kubectl apply -f project.yaml
```
## Create subnetes
```
kubectl apply -f subnet1.yaml
kubectl apply -f subnet2.yaml
```
## Create VNs
```
kubectl apply -f vn1.yaml
kubectl apply -f vn2.yaml
```
## Create shared RouteTarget
```
kubectl apply -f rt.yaml
```
## Add shared RT to RoutingInstances
```
kubectl patch routinginstances.core.contrail.juniper.net vn1 --patch "$(cat ri.yaml)"
kubectl patch routinginstances.core.contrail.juniper.net vn2 --patch "$(cat ri.yaml)"
```
## Create PODs
```
kubectl apply -f pod1.yaml
kubectl apply -f pod2.yaml
```
## Add routes to pods
```
kubectl exec pod1 -- ip route add 10.1.0.0/24 via 10.0.0.1
kubectl exec pod2 -- ip route add 10.0.0.0/24 via 10.1.0.1
```
## Ping
```
kubectl exec pod1 -- ping 10.1.0.3
kubectl exec pod2 -- ping 10.0.0.3
```
# ClusterIP
## Create Service
```
kubectl apply -f service.yaml
```
## Create frontend POD
```
kubectl apply -f frontend.yaml
```
## Create client POD
```
kubectl apply -f client.yaml
```
## Get ClusterIP
```
clusterIP=$(kubectl get svc frontend -o=jsonpath='{.spec.clusterIP}')
```
## curl from client
```
kubectl exec client -- curl ${clusterIP}
```

