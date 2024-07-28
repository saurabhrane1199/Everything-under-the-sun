# Kuberenetes

## Documentation

1. [kubectl documentation](https://kubectl.docs.kubernetes.io/references/)
2. 


## Local Cluster:

locacl kubernetes cluster is via minikube

### Commands

```bash
minikube start
```
```bash
minikube dashboard
```

## Deployment
It is a way of deploying and specifying pods but it helps to maintain replicas ensuring high availability. Deploying as kind:Pod doesn't make sense in production environment as we would always desire high availability

```yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
spec:
  replicas: 1
  selector:
    matchLabels:
      app: backend
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
      - name: backend
        image: saurabhrane1199/cattle-viz-backend:latest
        ports:
        - containerPort: 3001
```
### Service:

Service is how you actually orchestrate containers within the pod

```yaml

apiVersion: v1
kind: Service
metadata:
  name: backend
spec:
  selector:
    app: backend
  ports:
  - protocol: TCP
    port: 3001
    targetPort: 3001

```

