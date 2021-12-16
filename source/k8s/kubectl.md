# kubectl
- [overview of kubectl](https://kubernetes.io/docs/reference/kubectl/overview/)
- [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
- [kubectl Commands](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)
- [list of resource types](https://kubernetes.io/docs/reference/kubectl/overview/#resource-types)

## Display Resources
- all: `kubectl get all -A -o wide`
- custom resource definitions: `kubectl get crd`
- ingressroutes (custom resource definition from Traefik): `kubectl get ingressroutes -A`
- component statuses: `kubectl get cs`

## Create Resources
- expose deployment: `kubectl expose deploy <deployment_name> --port <port_number>` - [more](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#expose)

## Special Commands
- ececute bash on pod: `kubectl exec --stdin --tty <pod_name> -- /bin/bash`
- schedule Pods on the control-plane: `kubectl taint nodes --all node-role.kubernetes.io/master-`
