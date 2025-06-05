[Go Home](../README.md) | 
[Previous Step](./install-kubeadm.md) |
[Next Step](./install-calico-cni.md)
## 🚀 Step 8: Initialize the Kubernetes Cluster

Use the following command to initialize the cluster:

```bash
sudo kubeadm init \
  --pod-network-cidr=10.88.0.0/16 \
  --cri-socket=/run/containerd/containerd.sock \
  --ignore-preflight-errors=NumCPU \
  --v=5
```

### 🔍 Explanation of Flags

| Flag                               | Purpose                                                              |
| ---------------------------------- | -------------------------------------------------------------------- |
| `--pod-network-cidr`               | Specify the CIDR block for your pod network. Example: `10.88.0.0/16` |
| `--cri-socket`                     | Specify the container runtime socket (containerd in this case)       |
| `--ignore-preflight-errors=NumCPU` | Bypass the CPU check (useful for VMs with <2 CPUs)                   |
| `--v=5`                            | Set verbosity level for logs (useful for debugging)                  |

---

### 🧪 Example Output

Sample logs during execution:

```log
W0605 03:01:52.585417 initconfiguration.go:126] Usage of CRI endpoints without URL scheme is deprecated...
I0605 03:01:52.586190 interface.go:432] Looking for default routes...
[preflight] Running pre-flight checks
[WARNING NumCPU]: the number of available CPUs 1 is less than the required 2
...
[preflight] Some fatal errors occurred:
failed to create new CRI runtime service: validate service connection...
```

---

## 🧯 Troubleshooting: CRI Runtime Error

If you encounter the following error:

```log
[preflight] Some fatal errors occurred:
failed to create new CRI runtime service: ... rpc error: code = Unimplemented desc = unknown service runtime.v1.RuntimeService
```

It means there’s a **problem with the container runtime**. This could be due to:

* 🔧 Misconfiguration
* ❌ Inactive or failed service
* ⛔ Incompatibility

---

## ✅ Fixing the Container Runtime (containerd)

### 🧪 Step 1: Check if containerd is running

```bash
sudo systemctl status containerd
```

If it’s **inactive or failed**, restart it:

```bash
sudo systemctl restart containerd
```

---

### 🛠️ Step 2: Enable `SystemdCgroup` in containerd config

1. **Generate default config if not exists**:

```bash
sudo mkdir -p /etc/containerd
containerd config default | sudo tee /etc/containerd/config.toml
```

2. **Edit the config**:

```bash
sudo nano /etc/containerd/config.toml
```

3. **Update the following section**:
   Find:

```toml
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
  SystemdCgroup = false
```

Change to:

```toml
  SystemdCgroup = true
```

4. **Restart containerd**:

```bash
sudo systemctl restart containerd
```

---

Once fixed, rerun the `kubeadm init` command.

