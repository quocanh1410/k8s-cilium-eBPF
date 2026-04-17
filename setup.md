# k8s-cilium-eBPF
# Setup K8s with Cilium

# Dựng cụm k8s bằng kubeadm

**BƯỚC 1: Cài container runtime (containerd)**

```jsx
sudo apt update
sudo apt install -y containerd

sudo mkdir -p /etc/containerd
containerd config default | sudo tee /etc/containerd/config.toml

// bật systemd cgroup (RẤT QUAN TRỌNG)
sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml

sudo systemctl restart containerd
sudo systemctl enable containerd
```

**BƯỚC 2: Cài kubeadm, kubelet, kubectl**

```jsx
sudo apt install -y apt-transport-https ca-certificates curl

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt update
sudo apt install -y kubelet kubeadm kubectl

sudo apt-mark hold kubelet kubeadm kubectl
```

**BƯỚC 3: TẮT swap (bắt buộc)**

```jsx
sudo swapoff -a
sudo sed -i '/ swap / s/^/#/' /etc/fstab
```

**BƯỚC 4: Init cluster (KHÔNG kube-proxy)**

```jsx
sudo kubeadm init \
  --apiserver-advertise-address=192.168.122.24 \
  --skip-phases=addon/kube-proxy
```

**BƯỚC 5: Setup kubectl**

```jsx
mkdir -p ~/.kube
sudo kubectl config view --raw --kubeconfig=/etc/kubernetes/admin.conf > ~/.kube/config
sudo chown $USER:$USER ~/.kube/config

```

**BƯỚC 6: Cho phép schedule pod trên control-plane**

```jsx
kubectl taint nodes --all node-role.kubernetes.io/control-plane-
```

# Cài đặt Cilium eBPF thay thế kube-proxy

**BƯỚC 7: Cài Cilium (thay kube-proxy)**

```jsx
curl -LO https://github.com/cilium/cilium-cli/releases/latest/download/cilium-linux-amd64.tar.gz
sudo tar xzvf cilium-linux-amd64.tar.gz -C /usr/local/bin

//install
cilium install \
  --version 1.15.5 \
  --set kubeProxyReplacement=strict
//check 
cilium status

```

**Bước 8: Bật Envoy ( L7 )**

```jsx
cilium upgrade \
  --version 1.15.5 \
  --set envoy.enabled=true
```
