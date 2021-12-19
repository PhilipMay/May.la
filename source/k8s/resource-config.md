# Resource Configuration

## Deployment
- [Kubernetes doc: Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
- replicas can be omitted
- all three labels must exactly match
- `containerPort` is for information purposes only and can be omitted
- replicas defaults to 1 and can be omitted 

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: my-namespace
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```
- [Kubernetes doc: Service](https://kubernetes.io/docs/concepts/services-networking/service/)

## Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  namespace: my-namespace
  labels:
    app: nginx
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: nginx
```

## Namespace
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: my-namespace
---
```
