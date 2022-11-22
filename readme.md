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
Specifying the port that the client will use to communication with the server
```
python3 httpd.py client ip:port
```
`Now apply the previous process to all the clients at the end you should get an open terminal for each client`
Open new terminal `this time we will access the federated learning server (aggregator)
```
gnome-terminal
```
```
sudo kubectl exec -it server /bin/bash
```
```
cd ~/localfed/apps/experiments/
```
Now update the joining file that represent the client that will join the federated learning training round by adding `#client,ipaddress:port`
```
nano joinning.txt
```
Now Run the federated learning training process
```
python3 httpd.py server ip:port ./joinning.txt
```
Now on the master-node terminal deploy client03
```
kubectl apply -f ~/Desktop/client03.ymal
```
Check if client03 is runing and the ipaddress
```
kubectl get pods
```
```
kubectl get pod -o wide
```
Open new terminal
```
gnome-terminal
```
Now prepare client03 from the new terminal
```
sudo kubectl exec -it client03 /bin/bash
```
```
cd ~/localfed/apps/experiments/
```
```
python3 httpd.py client ip:port
```
Open new terminal and access the federated server
```
gnome-terminal
```
```
nano ~/localfed/apps/experiments/joinning.txt
```

`Add client03 to the list and you will se in the terminal of the runing federated learning server that client03 is now in the training process, also you will see in the terminal of client03 the communication messages`

