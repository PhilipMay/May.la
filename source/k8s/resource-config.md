# Resource Configuration

## Deployment
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
