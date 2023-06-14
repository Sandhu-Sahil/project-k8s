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
