[Go Home](../README.md) | 
[Previous Step](./Install-docker.md) |
[Next Step](./create-cluster-with-kubeadm.md)

## ☸️ Installing Kubernetes with `kubeadm` on CentOS

These are the official steps to set up a production-ready Kubernetes cluster using `kubeadm`.
Refer to the official documentation for updates:
🔗 [Kubernetes Installation Guide](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

---

### ❌ Step 1: Disable Swap

Kubernetes requires that swap is disabled.

Temporarily disable it:

```bash
sudo swapoff -a
```

To disable swap permanently, edit `/etc/fstab` and remove or comment out any swap entries:

```bash
sudo nano /etc/fstab
# Comment out any lines with 'swap'
```

---

### 🧱 Step 2: Install a Container Runtime

Kubernetes 1.24 and above **no longer support `dockerd`**. You should use **containerd** instead.

If you're using Docker, make sure you're on Kubernetes 1.23 or earlier, or configure Docker to use containerd as the runtime.

🔗 [Container runtimes supported by kubeadm](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)

---

### 🔐 Step 3: Disable SELinux (Set to Permissive Mode)

Set SELinux to permissive mode to prevent conflicts with Kubernetes components.

```bash
sudo setenforce 0
sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
```

---

### 📦 Step 4: Add the Kubernetes YUM Repository

Create the Kubernetes repo file:

```bash
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://pkgs.k8s.io/core:/stable:/v1.33/rpm/
enabled=1
gpgcheck=1
gpgkey=https://pkgs.k8s.io/core:/stable:/v1.33/rpm/repodata/repomd.xml.key
exclude=kubelet kubeadm kubectl cri-tools kubernetes-cni
EOF
```

---

### 📥 Step 5: Install `kubelet`, `kubeadm`, and `kubectl`

Install the required Kubernetes tools:

```bash
sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
```

Enable and start the `kubelet` service:

```bash
sudo systemctl enable --now kubelet
```

---

### ⚙️ Step 6: Configure `kubelet` cgroup Driver

You can configure the cgroup driver used by `kubelet` via `KubeletConfiguration`.

* Kubernetes v1.22+ defaults to `systemd` if not set.
* Kubernetes v1.28+ supports **automatic detection** (alpha feature).

You can explicitly set the driver by providing a configuration file during initialization:

```bash
cat <<EOF > kubeadm-config.yaml
apiVersion: kubeadm.k8s.io/v1beta3
kind: InitConfiguration
---
apiVersion: kubelet.config.k8s.io/v1beta1
kind: KubeletConfiguration
cgroupDriver: systemd
EOF
```

---

### 🚀 Step 7: Initialize the Kubernetes Cluster

Now, you can create the cluster using:

```bash
sudo kubeadm init --config=kubeadm-config.yaml
```

📌 After initialization, follow the printed instructions to:

* Set up `kubectl` for your user
* Deploy a network plugin (e.g., Calico, Flannel)

---

