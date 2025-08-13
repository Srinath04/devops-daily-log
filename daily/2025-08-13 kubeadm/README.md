
## 🔧 Activity
1. Created a Kubernetes cluster in AWS using kubeadm with:
	1 master node
	2 worker nodes
2. Configured all EC2 instances (Ubuntu 20.04) with:
3. containerd runtime
4. kubeadm, kubelet, kubectl installed
5. Initialized the cluster on the master node
6. Installed Weave Net as the CNI plugin
7. Joined both workers to the cluster
8. Verified node and pod status

## 🔍 Key Learnings
- Security Groups must allow inter-node communication for Kubernetes control plane and CNI ports:
	TCP 6443 (API server)
	TCP/UDP 6783, UDP 6784 (Weave Net)
	NodePort range TCP 30000–32767
	SSH TCP 22

- Using Elastic Network Interfaces (ENI) or fixed private IPs prevents cluster breakage after EC2 stop/start

- kubeadm init requires correct --pod-network-cidr based on CNI plugin choice

- Worker nodes require the join token from the master, which can be regenerated anytime with:

- Configure $HOME/.kube/config on the master for kubectl access

## 🖥 ️ Commands / Configs
```kubeadm token create --print-join-command``

### On all nodes (prereqs)
```
sudo apt update && sudo apt install -y apt-transport-https ca-certificates curl
sudo curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main

sudo apt update
sudo apt install -y kubelet kubeadm kubectl containerd
sudo systemctl enable kubelet
```
### On Control plane 
```
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
### Workers (join cluster)
```sudo kubeadm join <MASTER_PRIVATE_IP>:6443 --token <TOKEN> \
    --discovery-token-ca-cert-hash sha256:<HASH>```
### Verify
```
kubectl get nodes -o wide
kubectl get pods -A -o wide

kubectl create deployment nginx --image=nginx --replicas=2
kubectl get pods -o wide

kubectl expose deployment nginx \
  --type=NodePort \
  --port=8080 \
  --target-port=8080

kubectl get svc nginx

# From within pod:
kubectl run curlpod --image=radial/busyboxplus:curl -i --tty
curl http://nginx:80
exit
```
### Test from outside cluster (browser or local terminal)
```curl http://<ANY_OF_THREE_NODE_PUBLIC_IP>:32341```

### Ports Overview
```
Control Plane: TCP 6443
CNI (Weave): TCP/UDP 6783, UDP 6784
NodePort: TCP 30000–32767
SSH: TCP 22
```
