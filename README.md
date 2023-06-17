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
add the following line to the file
```
<port of service> <host name>
```
now you can access the service using the host name

Also, this ingress controller has a default backend, which is a service that handles all URL paths and hosts the ingress controller doesn't understand. Create a default backend service by:
```
kubectl apply -f default-backend.yaml
```
Which you can check by going to the host name you have given in the browser, you will see the default backend service running or by command:
```
kubectl describe ingress <ingress name> -n <namespace>
```

# multiple path for same host
you can use the same host name for multiple paths, for example, you can use the same host name for both mongo-express and mongo services, for that you have to use the same ingress name for both services, and add the following lines to the ingress.yaml file
```
spec:
  rules:
    - host: mongo-express.info
      http:
        paths:
          - path: /
            backend:
              serviceName: mongo-express-service
              servicePort: 8081
          - path: /mongo
            backend:
              serviceName: mongo-service
              servicePort: 27017
```
now you can access the mongo service by using the host name and path name, for example, mongo-express.info/mongo

# multiple host names for same path
you can use the same path for multiple host names, for example, you can use the same path for both mongo-express and mongo services, for that you have to use the same ingress name for both services, and add the following lines to the ingress.yaml file
```
spec:
  rules:
    - host: mongo-express.info
      http:
        paths:
          - path: /
            backend:
              serviceName: mongo-express-service
              servicePort: 8081
    - host: mongo.info
      http:
        paths:
          - path: /
            backend:
              serviceName: mongo-service
              servicePort: 27017
```
now you can access the mongo service by using the host name and path name, for example, mongo-express.info/ and mongo.info/

# configure ingress to use TLS
create a self-signed certificate
```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout tls.key -out tls.crt
```
create a secret using the certificate
```
kubectl create secret tls tls-secret --cert=tls.crt --key=tls.key
```
add the following lines to the ingress.yaml file
```
spec:
  tls:
    - hosts:
        - mongo-express.info
      secretName: tls-secret
  rules:
    - host: mongo-express.info
      http:
        paths:
          - path: /
            backend:
              serviceName: mongo-express-service
              servicePort: 8081
```
now you can access the mongo service by using the host name and path name, for example, mongo-express.info/

## or create a yaml file for secret
```
apiVersion: v1
kind: Secret
metadata:
  name: tls-secret
  namespace: default
data:
  tls.crt: <base64 encoded crt file>
  tls.key: <base64 encoded key file>
type: kubernetes.io/tls
```
and apply it by
```
kubectl apply -f <filename>
```