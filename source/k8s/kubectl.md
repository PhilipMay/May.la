# kubectl
- [Overview of kubectl](https://kubernetes.io/docs/reference/kubectl/overview/)
- [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
- [kubectl Commands](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)

## Display Resources
- display all: `kubectl get all -A -o wide`
- `kubectl get cs`

## Create Resources

## Special Commands
- schedule Pods on the control-plane: `kubectl taint nodes --all node-role.kubernetes.io/master-`
