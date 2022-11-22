## Kubernetes and Localfed commands
## Setup Kubernetes and Docker

1. Update the package list with the command:

```
sudo apt-get update
```
2. Next, install Docker with the command:
```
sudo apt-get install docker.io
```
3. Set Docker to launch at boot by entering the following:
```
sudo systemctl enable docker
```
```
sudo systemctl status docker
```
```
sudo systemctl start docker
```
4. Install Kubernetes
```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
```
```
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
```
```
sudo apt-get install kubeadm kubelet kubectl
```
```
sudo apt-mark hold kubeadm kubelet kubectl
```
###### Note: Change hostname optional (put it for example master-node on server and worker01 for client)
```
sudo hostnamectl set-hostname master-node
```
#Start Kubernetes Deployement
```
sudo swapoff -a
```
```
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```
```
mkdir -p $HOME/.kube
```
```
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
```
```
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
Deploy Pod Network to Cluster
```
sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```
Checking the existing nodes in the cluster
```
kubectl get nodes
```
check the exiting pods
```
kubectl get pods --all-namespaces
```
