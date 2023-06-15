# Encode into base 64

```
echo -n 'admin' | base64
```
```
echo -n 'password' | base64
```

# kubectl apply commands in order

```
kubectl apply -f mongo.secret.yaml
```
```
kubectl apply -f mongo.yaml
```
```
kubectl apply -f mongo.configmap.yaml
```
```
kubectl apply -f mongoExpress.yaml
```

# kubectl get commands

```
kubectl get pods
```
```
kubectl get secrets
```
```
kubectl get configmaps
```
```
kubectl get all
```
```
kubectl get pod -o wide
```
```
kubectl get pod --watch
```

# Assign external IP to service

```
minikube service mongo-express-service
```

# kubectl delete commands

```
kubectl delete -f mongo.secret.yaml
```
```
kubectl delete -f mongo.yaml
```
```
kubectl delete -f mongo.configmap.yaml
```
```
kubectl delete -f mongoExpress.yaml
```

# kubernetes namespaces

```
kubectl apply -f <filename> -n <namespace>
```
### or
```
kubectl create namespace <namespace>
```
and change the default namespace by using kubectx and kubens,
select namespace by :
```
kubens <namespace>
```
### or
change namespace in the yaml file
```
metadata:
  name: mongo-express
  namespace: <namespace>
```

# kubernetes ingress
start ingress controller minikupe addon
```
minikube addons enable ingress
```
```
kubectl get pods -n kube-system
```
you can see the ingress controller running, now apply the ingress rules <br> make a yaml file to apply, you can use namespace or directly apply to the default namespace, both cases are shown in ingress.yaml file
```
kubectl apply -f ingress.yaml
```
```
kubectl get ingress
```
now add host to link the ingress to the service
```
sudo gedit /etc/hosts
```
