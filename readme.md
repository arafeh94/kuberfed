## Kubernetes and Docker commands
## Install Kubernetes and Docker

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
## Start Kubernetes Deployement (server side)
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
Check the exiting pods
```
kubectl get pods --all-namespaces
```
Get the token needed to join the cluster
```
sudo kubeadm token create --print-join-command
```
###### ouput `kubeadm join 10.0.2.15:6443 --token sm1i4p.v57qcs7ppiav6y53 --discovery-token-ca-cert-hash sha256:402c7635040ea5b2dd5f73622c0b1bdedd96c09831463ce70dd7b2cdcf957eed`
## Now on the Worker side
1. First swapoff
```
sudo swapoff -a
```
2. Now Run the Joining key as root
```
sudo kubeadm join 10.0.2.15:6443 --token sm1i4p.v57qcs7ppiav6y53 --discovery-token-ca-cert-hash sha256:402c7635040ea5b2dd5f73622c0b1bdedd96c09831463ce70dd7b2cdcf957eed 
```
## GO BACK TO MASTER-NODE
Check if the worker appears in the cluster
```
kubectl get nodes
```
## Start to Deploy Clients
First Check the .ymal files for deployment

`This one represents the federated server aggregator that take place on the master-node machine along with the kubernetes`
```
kubectl apply -f ~/Desktop/server.ymal
```
`client01 deployment`
```
kubectl apply -f ~/Desktop/client01.ymal
```
`client02 deployment`
```
kubectl apply -f ~/Desktop/client02.ymal
```
Now check if all the pods are running normally
```
kubectl get pods
```
To get the ipaddress of each pod (client)
```
kubectl get pod -o wide
```
## Now moving to the Federated learning part
Open new terminal
```
gnome-terminal
```
Accessing client01
```
sudo kubectl exec -it client01 /bin/bash
```
Preparing client01
```
cd ~/localfed/apps/experiments/
```
Execute the experiments. Replace with relevant IPs
```
mpirun -np 3 -host 127.0.0.1,10.244.1.2,10.244.1.2 python3 distributed_averaging.py
```