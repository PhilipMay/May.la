# Kubernetes HA Installation
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/
- https://projectcalico.docs.tigera.io/about/about-calico

## Steps
Check if swap is disabled: `cat /etc/fstab`

see https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#letting-iptables-see-bridged-traffic
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

see https://kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd
```bash
cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF

modprobe overlay

modprobe br_netfilter

cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF

sudo sysctl --system
```

see https://docs.docker.com/engine/install/debian/
```bash
apt-get update

apt-get install -y ca-certificates curl gnupg lsb-release

curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

apt-get update

apt install apparmor

apt-get install containerd.io

apt-mark hold containerd.io

mkdir -p /etc/containerd

containerd config default > /etc/containerd/config.toml

systemctl restart containerd
```

- see: https://kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd-systemd
- edit `/etc/containerd/config.toml`
- add the 2nd last line below 1st line:

```text
  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
    SystemdCgroup = true
```

- restart containerd: `systemctl restart containerd`

see https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#installing-kubeadm-kubelet-and-kubectl
```bash
apt-get update

apt-get install -y apt-transport-https ca-certificates curl

curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg

echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

apt-get update

apt-get install -y kubelet kubeadm kubectl

apt-mark hold kubelet kubeadm kubectl
```

- install and start firewall - double check internal network adapter (enp7s0 or ens10)
- disable ipv6

see 
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#initializing-your-control-plane-node
- https://projectcalico.docs.tigera.io/getting-started/kubernetes/quickstart#create-a-single-host-kubernetes-cluster
```bash
kubeadm init --control-plane-endpoint="<external-ip-of-load-balancer>:6443" --upload-certs --apiserver-advertise-address=<internal-ip> --pod-network-cidr=192.168.0.0/16

# then add more master nodes with (always manualy add --apiserver-advertise-address=<internal-ip>):
  kubeadm join <external-ip-of-load-balancer>:6443 --token <token> \
	--discovery-token-ca-cert-hash sha256:<hash> \
	--control-plane --certificate-key <cert-key> --apiserver-advertise-address=<internal-ip>
```

- edit `/var/lib/kubelet/kubeadm-flags.env`
- append `--node-ip=<your-internal-ip>`
- reboot
- see [Kubernetes doc: Workflow when using kubeadm init](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/kubelet-integration/#workflow-when-using-kubeadm-init) and [Stack Overflow](https://stackoverflow.com/a/57506203/271118)

- copy config

```bash
mkdir ~/.kube
scp root@<master_ip_or_hostname>:/etc/kubernetes/admin.conf ~/.kube/config
```

- untaint master: `kubectl taint nodes --all node-role.kubernetes.io/master-`

## Install Calico
- see https://projectcalico.docs.tigera.io/getting-started/kubernetes/self-managed-onprem/onpremises#install-calico-with-kubernetes-api-datastore-50-nodes-or-less
- [Helm install](https://projectcalico.docs.tigera.io/getting-started/kubernetes/helm)

```bash
helm repo add projectcalico https://docs.projectcalico.org/charts
helm repo update
helm install calico projectcalico/tigera-operator
```

## Opional: Install metrics server
- see https://github.com/kubernetes-sigs/metrics-server
- see https://artifacthub.io/packages/helm/metrics-server/metrics-server
- `helm install metrics-server metrics-server/metrics-server -n metrics-server --create-namespace -f metrics-server-helm-values.yaml`

## Install Longhorn
- see https://longhorn.io/docs/1.2.2/deploy/install/install-with-helm/
- do on all nodes: `apt-get install -y open-iscsi nfs-common`
- `helm install longhorn longhorn/longhorn --namespace longhorn-system --create-namespace`

## Network Debug
- see open ports and more:
  - `lsof -i -P -n | grep LISTEN`
  - `lsof -i -P -n | grep kubectl`

## Next Steps
- untaint master: `kubectl taint nodes --all node-role.kubernetes.io/master-`
- install helm: https://helm.sh/docs/intro/install/#from-apt-debianubuntu
- install traefik: https://doc.traefik.io/traefik/getting-started/install-traefik/#use-the-helm-chart

```yaml
service:
  externalIPs:
    - <the_external_ip>
```

```bash
helm repo add traefik https://helm.traefik.io/traefik
helm repo update
helm install traefik traefik/traefik -n traefik --create-namespace -f traefik-helm-values.yaml
```

- expose the traefik dashboard:
  - `k port-forward <traefik_pod> 9000:9000 -n traefik --address 0.0.0.0`
  - http://<ip>:9000/dashboard/

## Join new Nodes to an existing Cluster
- show token: `kubeadm token list`
- create new token: `kubeadm token create`
- print join command: `kubeadm token create --print-join-command`
