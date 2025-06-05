[Go Home](../README.md)
# 🧭 Kubernetes Single-Node Cluster Setup (Self-Hosted)

This section documents the step-by-step instructions for setting up a single-node Kubernetes cluster using `kubeadm`. It includes:

- Installing Docker
- Installing and configuring `kubeadm`, `kubelet`, and `kubectl`
- Initializing the Kubernetes control plane
- Installing Calico as the Container Network Interface (CNI)

---

## 📁 Guide Index


| Step | Description                             | Link                                                     |
| ---- | --------------------------------------- | -------------------------------------------------------- |
| 1️⃣  | Instructions for installing Docker       | [Install Docker](./docs/Install-docker.md) |
| 2️⃣  | How to install `kubeadm`, `kubelet`, and `kubectl` | [Install Kubeadm](./docs/install-kubeadm.md) |
| 3️⃣  | How to initialize the control plane using `kubeadm` | [Initialize a Cluster](./docs/create-cluster-with-kubeadm.md) |
| 4️⃣  | How to install Calico to enable pod-to-pod networking | [Install Calico CNI](./docs/install-calico-cni.md) |


---

## 🔧 Prerequisites

Before beginning, ensure:

- You are running a supported Linux OS (e.g., CentOS, Ubuntu)
- You have root access or `sudo` privileges
- Swap is disabled (`swapoff -a`)
- Required ports (e.g., 6443) are open if behind a firewall

---

## 🛠️ What You'll Get

By following these instructions, you'll have:

- A **self-managed, single-node Kubernetes cluster**
- Pods able to communicate via **Calico CNI**
- Core system components like **CoreDNS**, **kube-proxy**, and **etcd** running and healthy

---

## 📦 Technologies Used

- [Docker](https://docs.docker.com/)
- [Kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/)
- [Calico CNI](https://docs.tigera.io/calico/latest/getting-started/kubernetes/self-managed-onprem/onpremises)

---

## ✅ Final Verification

After completing all steps, verify the cluster is healthy:

```bash
kubectl get nodes
kubectl get pods -A
````

You should see:

* Node status: `Ready`
* CoreDNS and Calico pods: `Running`

```
