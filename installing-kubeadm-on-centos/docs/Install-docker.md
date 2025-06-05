[Go Home](../README.md) | 
[Next Step](./install-kubeadm.md)

## 📦 Installing Docker (CentOS)

Docker is required as the container runtime for your Kubernetes nodes. Follow these steps to install Docker on CentOS using the official Docker repository.

### 🔗 Reference

Official Docker installation guide: [https://docs.docker.com/engine/install/centos](https://docs.docker.com/engine/install/centos/)

---

### 🛠️ Step 1: Set up the Docker repository

Install the `dnf-plugins-core` package to manage your DNF repositories, then add the Docker repo.

```bash
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

---

### 🐳 Step 2: Install Docker Engine

Install the latest version of Docker and its components:

```bash
sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

✅ **GPG Key Verification**
If prompted to accept the GPG key, verify that the fingerprint matches:

```
060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35
```

Then, proceed to accept it.

---

### ▶️ Step 3: Start and Enable Docker

Enable the Docker service to start on boot and run it immediately:

```bash
sudo systemctl enable --now docker
```

This will:

* Start the Docker daemon
* Enable it to launch automatically at system boot
* Create the `docker` group (note: it does **not** add your user to it by default)

---
Check Docker’s status to confirm it’s running properly:

```bash
systemctl status docker
```

---

### 👤 Step 4: Add Your User to the Docker Group (Optional but Recommended)

To avoid having to use `sudo` with every Docker command:

```bash
sudo usermod --group docker --append $USER
```

⚠️ **Note:** You need to log out and log back in (or reboot) for this change to take effect.

---