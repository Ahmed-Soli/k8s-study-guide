[Go Home](../README.md) | 
[Previous Step](./create-cluster-with-kubeadm.md)

## 🌐 Install a Pod Network Add-on (CNI) – Using Calico

### ⚠️ Why You Must Install a CNI Plugin

* Kubernetes does **not** install a CNI (Container Network Interface) plugin by default.
* Without a CNI, **Pods cannot communicate**, and critical components like **CoreDNS will remain in `Pending` status**.
* You **must** install a CNI before your cluster is fully functional.

> 📌 Note: Ensure your `--pod-network-cidr` (used in `kubeadm init`) **does not overlap** with host network ranges. In our setup, we used `--pod-network-cidr=10.88.0.0/16`.

---

### ✅ We Will Use: Calico (Self-Managed, On-Premises)

Reference: [Calico Docs – Self-Managed On-Premises](https://docs.tigera.io/calico/latest/getting-started/kubernetes/self-managed-onprem/onpremises)

---

### 🧰 Step-by-Step Installation

1. **Download the Calico manifest**:

   ```bash
   curl -O https://raw.githubusercontent.com/projectcalico/calico/v3.30.1/manifests/calico.yaml
   ```

2. **Customize the manifest (Optional)**:

   * If you're using a pod CIDR **other than `192.168.0.0/16`**, Calico usually **detects it automatically** from the Kubernetes config.
   * For other platforms or to be explicit:

     * Edit the manifest:

       ```bash
       sudo nano calico.yaml
       ```
     * Uncomment and set:

       ```yaml
       - name: CALICO_IPV4POOL_CIDR
         value: "10.88.0.0/16"
       ```

3. **Apply the manifest**:

   ```bash
   kubectl apply -f calico.yaml
   ```

---

### 🔎 Before Installing Calico

Check pod status:

```bash
kubectl get pod -n kube-system
```

Expected output:

```text
coredns-xxx   0/1   Pending
...
```

CoreDNS stays `Pending` until networking is ready.

---

### ✅ After Installing Calico

```bash
kubectl get all -n kube-system
```

Expected output:

```text
pod/calico-kube-controllers-xxx   1/1   Running
pod/calico-node-xxx               1/1   Running
pod/coredns-xxx                   1/1   Running
...
```

One of the two CoreDNS pods might still be `Pending` — this is usually temporary and should resolve shortly.

---

### ✅ Network Components Overview (Post-Calico)

| Component                 | Status                   | Description                         |
| ------------------------- | ------------------------ | ----------------------------------- |
| `calico-node`             | ✅ Running                | Handles pod networking on each node |
| `calico-kube-controllers` | ✅ Running                | Manages Calico configuration        |
| `coredns`                 | ✅ Running (1/2 at least) | DNS for service discovery           |
| `kube-proxy`              | ✅ Running                | Handles service networking rules    |

---
