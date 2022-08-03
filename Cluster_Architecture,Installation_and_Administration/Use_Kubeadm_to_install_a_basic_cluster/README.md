# Cluster Architecture, Installation & Configuration - 

## Objective
*  Use Kubeadm to install a basic cluster

---

### Implementation

### Useful Information
* After installing Cri-o change the "ExecStart" from /usr/bin/local/crio to /usr/bin/cio
* 

### Useful Commands

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

---
