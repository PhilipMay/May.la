# Kubernetes Installation
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

I did a reboot here - just to be sure

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

see 
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#initializing-your-control-plane-node
- https://projectcalico.docs.tigera.io/getting-started/kubernetes/quickstart#create-a-single-host-kubernetes-cluster
```bash
kubeadm init --pod-network-cidr=192.168.0.0/16 --control-plane-endpoint=159.69.24.59

export KUBECONFIG=/etc/kubernetes/admin.conf
```

## Install Calico
- see https://projectcalico.docs.tigera.io/getting-started/kubernetes/self-managed-onprem/onpremises#install-calico-with-kubernetes-api-datastore-50-nodes-or-less

```text
curl https://docs.projectcalico.org/manifests/calico.yaml -O

kubectl apply -f calico.yaml
```

## Fix unhealthy scheduler
When you execute `kubectl get cs` you see that scheduler is unhealthy. This is the output:
```text
NAME                 STATUS      MESSAGE                                                                                       ERROR
scheduler            Unhealthy   Get "http://127.0.0.1:10251/healthz": dial tcp 127.0.0.1:10251: connect: connection refused   
controller-manager   Healthy     ok                                                                                            
etcd-0               Healthy     {"health":"true","reason":""} 
```

To fix this (see https://stackoverflow.com/a/66635130/271118) edit `/etc/kubernetes/manifests/kube-scheduler.yaml` and remove (or add `#` in front of) `- --port=0` at the location "spec -> containers -> command -> kube-scheduler". After that restart the kubelet service: `systemctl restart kubelet.service`

## Error when apparmor is missing
Now I am getting this error:
```text
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
[kubelet-check] Initial timeout of 40s passed.

	Unfortunately, an error has occurred:
		timed out waiting for the condition

	This error is likely caused by:
		- The kubelet is not running
		- The kubelet is unhealthy due to a misconfiguration of the node in some way (required cgroups disabled)

	If you are on a systemd-powered system, you can try to troubleshoot the error with the following commands:
		- 'systemctl status kubelet'
		- 'journalctl -xeu kubelet'

	Additionally, a control plane component may have crashed or exited when started by the container runtime.
	To troubleshoot, list all containers using your preferred container runtimes CLI.

	Here is one example how you may list all Kubernetes containers running in cri-o/containerd using crictl:
		- 'crictl --runtime-endpoint /run/containerd/containerd.sock ps -a | grep kube | grep -v pause'
		Once you have found the failing container, you can inspect its logs with:
		- 'crictl --runtime-endpoint /run/containerd/containerd.sock logs CONTAINERID'

error execution phase wait-control-plane: couldn't initialize a Kubernetes cluster
To see the stack trace of this error execute with --v=5 or higher
```

`journalctl -u kubelet` provides this:
```text
[...] kubelet.go:2337] "Container runtime network not ready" networkReady="NetworkReady=false reason:NetworkPluginNotReady message:Network plugin returns error: cni plugin not initialized"
```

`journalctl -u containerd` provides this:
```text
[...] containerd[4018]: time="2021-12-07T08:02:36.964061674Z" level=error msg="failed to load cni during init, please check CRI plugin status before setting up network for pods" error="cni config load failed: no network config found in /etc/cni/net.d: cni plugin not initialized: failed to load cni config"
```

`journalctl -xeu kubelet` provides this:
```text
Dec 07 08:32:57 master1 kubelet[8864]: E1207 08:32:57.747558    8864 kubelet_node_status.go:93] "Unable to register node with API server" err="Post \"https://159.69.24.59:6443/api/v1/nodes\": dial tcp 159.69.24>
Dec 07 08:32:57 master1 kubelet[8864]: E1207 08:32:57.777954    8864 kubelet.go:2412] "Error getting node" err="node \"master1\" not found"
Dec 07 08:32:57 master1 kubelet[8864]: E1207 08:32:57.878672    8864 kubelet.go:2412] "Error getting node" err="node \"master1\" not found"
Dec 07 08:32:57 master1 kubelet[8864]: E1207 08:32:57.979180    8864 kubelet.go:2412] "Error getting node" err="node \"master1\" not found"
[...]
Dec 07 08:33:00 master1 kubelet[8864]: E1207 08:33:00.217568    8864 kubelet.go:2337] "Container runtime network not ready" networkReady="NetworkReady=false reason:NetworkPluginNotReady message:Network plugin r>
Dec 07 08:33:00 master1 kubelet[8864]: E1207 08:33:00.297172    8864 kubelet.go:2412] "Error getting node" err="node \"master1\" not found"
Dec 07 08:33:00 master1 kubelet[8864]: E1207 08:33:00.398036    8864 kubelet.go:2412] "Error getting node" err="node \"master1\" not found"
Dec 07 08:33:00 master1 kubelet[8864]: E1207 08:33:00.498572    8864 kubelet.go:2412] "Error getting node" err="node \"master1\" not found"
[...9
```

containerd seems to complain about apparmor: `error="failed to create containerd container: get apparmor_parser version: exec: \"apparmor_parser\": executable file not found in $PATH"`

I did try: `apt install apparmor apparmor-utils`

Later saw that `apt install apparmor` is enough.

Why is the CNI plugin not installed?!

## Network Debug
- see open ports and more:
  - `lsof -i -P -n | grep LISTEN`
  - `lsof -i -P -n | grep kubectl`

## Next Steps
- untaint master: `kubectl taint nodes --all node-role.kubernetes.io/master-`
- install helm: https://helm.sh/docs/intro/install/#from-apt-debianubuntu
- install traefik: https://doc.traefik.io/traefik/getting-started/install-traefik/#use-the-helm-chart
  - `helm install traefik traefik/traefik -n traefik --create-namespace` 
- expose the traefik dashboard:
  - `k port-forward <traefik_pod> 9000:9000 -n traefik --address 0.0.0.0`
  - http://<ip>:9000/dashboard/