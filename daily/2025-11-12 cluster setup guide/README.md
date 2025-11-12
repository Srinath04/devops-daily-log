
#### Self Managed Kubernetes Cluster Setup on GCP

##### Prerequisites

Before starting, ensure the following:
1. **gcloud CLI** is installed and configured on your workstation. 
2. My default **project**, **region**, and **zone** are already configured, so they may not appear explicitly in the commands.
Adjust or include them as needed based on your own infrastructure setup.
3. A **VPC**, **subnet** and **Three VM instances** one for **Control Plane (Master)** and two for **Worker Nodes** are created next.
###### These steps (1–4) can also be performed easily via the **GCP Console (UI)**

###### 1. Create a VPC  
`gcloud compute networks create example-k8s --subnet-mode custom`

###### 2. Create Subnets
```
gcloud compute networks subnets create k8s-nodes \
--network example-k8s \
--range 10.240.0.0/24
```
###### 3. Create two firewall rules
internal
```
gcloud compute firewall-rules create example-k8s-allow-internal \
  --network example-k8s \
  --allow tcp:10250,tcp:10256,tcp:179,tcp:5473,udp:4789,tcp:30000-32767,ipip \
  --source-ranges 10.240.0.0/24 \
  --enable-logging \
```

external
```
gcloud compute firewall-rules create example-k8s-allow-external \
  --network=example-k8s \
  --allow tcp:22,tcp:6443,tcp:2379-2380,tcp:10250,tcp:10257,tcp:10259 \ 
  --source-ranges=0.0.0.0/0 \
  --enable-logging
```

###### 4.Create VM instances
on Controlplane 
```
gcloud compute instances create controller \                            
  --async \
  --can-ip-forward \
  --image-family=ubuntu-2204-lts \
  --image-project=ubuntu-os-cloud \
  --machine-type=e2-standard-2 \
  --private-network-ip=10.240.0.33 \
  --scopes=compute-rw,storage-ro,service-management,service-control,logging-write,monitoring \
  --subnet=k8s-nodes \
  --tags=example-k8s,controller
```

on worker node 1 and 2 
```
for i in 0 1; do
  gcloud compute instances create worker-${i} \
  --async \
  --can-ip-forward \
  --image-family ubuntu-2204-lts \
  --image-project ubuntu-os-cloud \
  --machine-type e2-standard-2 \
  --private-network-ip 10.240.0.2${i} \
  --scopes compute-rw,storage-ro,service-management,service-control,logging-write,monitoring \
  --subnet k8s-nodes \
  --tags example-k8s,worker
done
```

when your virtual machines (1 controlplane node and 2 worker nodes are ready)
proceed to login and setup the server

---
Control Plane (Master Node) Setup

**SSH into the Control Plane node** and perform the following steps (Step 1 to Step 10):

`gcloud compute ssh controller`
#### 1. Disable Swap
Run below commands to disable swap:

```shell
swapoff -a
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
```

### 2. Configure IPv4 Forwarding and iptables for Bridged Traffic

Load necessary modules and apply system parameters:
```shell
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter
```

Setup sysctl parameters
```shell
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

sudo sysctl --system
```

Verify the modules and parameters:
```shell
lsmod | grep br_netfilter
lsmod | grep overlay
sysctl net.bridge.bridge-nf-call-iptables net.bridge.bridge-nf-call-ip6tables net.ipv4.ip_forward
```
### 3. Install Container Runtime

```
curl -LO https://github.com/containerd/containerd/releases/download/v2.1.4/containerd-2.1.4-linux-amd64.tar.gz

sudo tar Cxzvf /usr/local containerd-2.1.4-linux-amd64.tar.gz

curl -LO https://raw.githubusercontent.com/containerd/containerd/main/containerd.service

sudo mkdir -p /usr/local/lib/systemd/system/
sudo mv containerd.service /usr/local/lib/systemd/system/
```

Set up the default containerd configuration:
`
```
sudo mkdir -p /etc/containerd

containerd config default | sudo tee /etc/containerd/config.toml
sudo sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' /etc/containerd/config.toml
```

Enable and start the containerd service:

```
sudo systemctl daemon-reload
sudo systemctl enable --now containerd
systemctl status containerd
```

### 4. Install runc

```
curl -LO https://github.com/opencontainers/runc/releases/download/v1.2.7/runc.amd64
sudo install -m 755 runc.amd64 /usr/local/sbin/runc

```

### 5. Install CNI Plugin

```
curl -LO https://github.com/containernetworking/plugins/releases/download/v1.7.1/cni-plugins-linux-amd64-v1.7.1.tgz

sudo mkdir -p /opt/cni/bin

sudo tar -xvf cni-plugins-linux-amd64-v1.7.1.tgz -C /opt/cni/bin/
```

### 6. Install kubeadm, kubelet, and kubectl

Update packages and install Kubernetes tools:

```
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl gpg

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
```

Verify version if required, before installation
`list -a kubeadm kubelet kubectl`

```
sudo apt-get install -y kubelet=1.33.0-1.1 kubeadm=1.33.0-1.1 kubectl=1.33.0-1.1 --allow-downgrades --allow-change-held-packages

sudo apt-mark hold kubelet kubeadm kubectl

sudo systemctl enable --now kubelet

```

Verify the installation:
```shell
kubeadm version
kubelet --version
kubectl version --client
```
### 7. Configure crictl for containerd

```
sudo crictl config runtime-endpoint unix:///var/run/containerd/containerd.sock
```

verify the configuration
`sudo crictl info`

### 8. Initialize the Control Plane (Only on Controlplane node)

Run the following command to initialize the Kubernetes control plane:
```
sudo kubeadm init --pod-network-cidr=192.168.0.0/16 --apiserver-advertise-address=10.240.0.33 --node-name controller
```

or just run below command as it can automatically detuct and advertise address

```
sudo kubeadm init --pod-network-cidr=192.168.0.0/16 --node-name controller
```

> **Note:** Save the generated **join command** to add worker nodes later.
### 9. Configure kubeconfig

Prepare kubeconfig for kubectl:

```shell
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### 10. Install Calico Network Plugin

```
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.31.0/manifests/operator-crds.yaml

kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.31.0/manifests/tigera-operator.yaml

curl https://raw.githubusercontent.com/projectcalico/calico/v3.31.0/manifests/custom-resources.yaml -O

kubectl apply -f custom-resources.yaml
```

---
Worker Node Setup

**For each Worker Node:**

- Repeat **Steps 1 to 7** from the Control Plane setup.
- Execute the saved join command after step 7 in each of worker node to join into the cluster

```
sudo kubeadm join 10.240.0.33:6443 --token qxixxx --discovery-token-ca-cert-hash sha256:53xxadx
```
   
Its possible to regenerate join command in controlplane node if missed, using   

```
kubeadm token create --print-join-command
```

---
**Finally:**  

Verify nodes, kube-system pods (apiserver/controller/scheduler/etcd), kube-proxy DaemonSet, CoreDNS, services & endpoints, etcd health, kubelet/runtime status, and open control-plane ports.

_which should all be fine if everything’s wired up correctly._

Add your favourite **aliases**, **Kubernetes autocompletion**, and other handy tools to your `~/.bashrc`.



Go ahead and **rock your cluster! 🚀**

---
