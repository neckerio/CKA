# Cluster Architecture, Installation & Configuration - 

## Objective
*  Use Kubeadm to install a basic cluster

---

### Implementation

### Useful Information
* After installing Cri-o change the "ExecStart" from /usr/bin/local/crio to /usr/bin/cio
* Set **--control-plane-endpoint** to set the shared endpoint for all control-plane nodes, when upgrading to High Availability
* Turning a single control plane cluster created without **--control-plane-endpoint** into a highly available cluster is not supported by kubeadm.
* By default, your cluster will not schedule Pods on the control plane nodes for security reasons. If you want to be able to schedule Pods on the control plane nodes, for example for a single machine Kubernetes cluster, run:

```zsh
kubectl taint nodes --all node-role.kubernetes.io/control-plane- node-role.kubernetes.io/master-
```

* If you want to connect to the API Server from outside the cluster you can use kubectl proxy:

```zsh
scp root@<control-plane-host>:/etc/kubernetes/admin.conf .
kubectl --kubeconfig ./admin.conf proxy
```

* You can now access the API Server locally at http://localhost:8001/api/v1

#### initialize Control Plane
* --control-plane-endpoint=<ip-address>:6443 -- for high availability
* --cri-socket=unix:///var/run/crio/crio.sock -- socket will vary depending on CRI
* --apiserver-advertise-address=<ip-address> -- specify IP
* ensure "Forwarding IPv4 and letting iptables see bridged traffic" are set[^runtime_prerequisites]
* set systemd cgroup driver; CRI-O uses it by default[^runtime_prerequisites]

#### kubeadm init & cni install
* Disable all swap, comment out the /etc/fstab line to persist
* (optional) Parallelize token distribution for easier automation, this option has relaxed security guarantees because it doesn't allow the root CA hash to be validated with --discovery-token-ca-cert-hash

### Useful Commands
* kubeadm init (--skip-phases=addon/kube-proxy) for cilium
* kubeadm reset
* kubeadm token (list)
* kubeadm join

### Useful Directories/Files
* /etc/systemd/system/cri-o.service
* /var/run/crio/crio.sock
* /etc/crio/crio.conf
* /etc/crio/crio.conf.d/(drop in .conf)

### Useful Packages
* apt-transport-https
* ca-certificates
* curl
* kubelet
* kubectl
* kubeadm
* cri-o
* cri-o-runc (runtime, didn't work defaulted to regular runc)
* runc

---

## Notes
* Instructions to [Install Kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/) 
* Instructions to [Install Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)
* Instructions to [Install Cri-o](https://github.com/cri-o/cri-o/blob/main/install.md)
* Instructions to [Create a Kubeadm cluster](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/) [^runtime_prerequisites]
* Cluster Networking, [How to implement the Kubernetes Networking Model](https://kubernetes.io/docs/concepts/cluster-administration/networking/#how-to-implement-the-kubernetes-networking-model)
* [Network Plugins](https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/#cni) from Kubernetes site
* CNI on [github](https://github.com/containernetworking/cni)
* [Cilium](https://github.com/cilium/cilium) on github
* Instruction to [install Cilium](https://docs.cilium.io/en/stable/gettingstarted/) and [installation using Kubeadm](https://docs.cilium.io/en/stable/gettingstarted/k8s-install-kubeadm/) are in the Advanced installation settings, as well as installing [Kubernetes without Kubeproxy](https://docs.cilium.io/en/stable/gettingstarted/kubeproxy-free/#kubeproxy-free)

---
