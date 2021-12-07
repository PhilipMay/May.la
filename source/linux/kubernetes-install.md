# Kubernetes Installation
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/
- https://projectcalico.docs.tigera.io/about/about-calico

## Steps
Check if swap is disabled: `cat /etc/fstab`

```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sysctl --system
```
