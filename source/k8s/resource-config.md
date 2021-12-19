# Resource Configuration

## Deployment
- [Kubernetes doc: Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
- `replicas` 
  - can be omitted
  - defaults to 1
- all three labels must exactly match
- `containerPort` is for information purposes only and can be omitted

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

## Service
- [Kubernetes doc: Service](https://kubernetes.io/docs/concepts/services-networking/service/)
- `spec.selector.app` must reference the label(-s) of the deployment (pod)
- `targetPort` is the port of the pod
- `port` is the port of the service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  namespace: my-namespace
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: nginx
```

## Secret

## PersistentVolumeClaim

## IngressRoute
- [Traefik doc: Kind: IngressRoute](https://doc.traefik.io/traefik/routing/providers/kubernetes-crd/#kind-ingressroute)

## Middleware
- [Traefik doc: Kind: Middleware](https://doc.traefik.io/traefik/routing/providers/kubernetes-crd/#kind-middleware)

## Namespace
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: my-namespace
```
