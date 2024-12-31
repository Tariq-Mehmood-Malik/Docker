# Kubernetes Cluster Creation

## Verify the prerequisites
-----------------------------------
- 1. Have 2 Linux nodes with Ubuntu OS
- 2. 2 GB or more of RAM per machine 
- 3. 2 CPUs or more.
- 4. Full network connectivity between all machines in the cluster.
     -- we can check with ping command in each node
- 5. Certain ports are open on your machines. See https://kubernetes.io/docs/reference/networking/ports-and-protocols/ for more details.
**Master Node**
```bash
sudo iptables -A INPUT -p tcp --dport 6443 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 2379:2380 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 10250 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 10251 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 10252 -j ACCEPT
```

**Worker Node**
```bash
sudo iptables -A INPUT -p tcp --dport 10250 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 10256 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 30000:32767 -j ACCEPT
```


- 6. Disable swap in each node.We can do either of below steps for this.
```bash
sudo swapoff -a 
(crontab -l 2>/dev/null; echo "@reboot /sbin/swapoff -a") | crontab - || true
```

- 7. Enable ip-forwarding on all the Nodes
```bash
# sysctl params required by setup, params persist across reboots
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.ipv4.ip_forward                 = 1
EOF
```

#### Apply sysctl params without reboot
```bash
sudo sysctl --system
```
#### Verify that net.ipv4.ip_forward is set to 1 with:
```bash
sysctl net.ipv4.ip_forward
```

## All Nodes
Install Container Runtime (conatinerd)

#### Add Docker's official GPG key:
```bash
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

#### Add the repository to Apt sources:
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```bash
sudo apt-get update
```
```bash
sudo apt-get install containerd.io
```

verify whether containerd is up and running with below command.
```bash
systemctl status containerd
```

Configure containerd to use systemd cgroup driver.
Open /etc/containerd/config.toml and add below. Also comment out disabled_plugins in the same file.
```bash
sudo nano /etc/containerd/config.toml
```
```yaml
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
    SystemdCgroup = true
```

Restart Containerd
```bash
sudo systemctl restart containerd

systemctl status containerd
```

Install kubeadm, kubelet and kubectl
-------------------------------------------------------------------
Execute below commands in all nodes to install  kubeadm, kubelet and kubectl
```bash
sudo apt-get update
```
#### apt-transport-https may be a dummy package; if so, you can skip that package
```bash
sudo apt-get install -y apt-transport-https ca-certificates curl gpg
```

#### If the directory `/etc/apt/keyrings` does not exist, it should be created before the curl command, read the note below.
```bash
sudo mkdir -p -m 755 /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.32/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```
```bash
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.32/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```
```bash
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
sudo systemctl enable --now kubelet
```

Setup the Control plane node
-----------------------------------------------------------------------
In the Control plane(master) node, Execute below steps.
```bash
kubeadm init --apiserver-advertise-address <your-ip> --pod-network-cidr 10.244.0.0/16
```

Copy the output of above command to some textpad/notepad.
Now execute below commands as in output with a OS user which we want to access k8s cluser in master node
```bash
   mkdir -p $HOME/.kube
   sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
   sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
Install CNI plugin(flannel)
---------------------------------------------------
In controle plane node execute below comamnds.
```bash
wget https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml
```

Update below in kube-flannel.yml. The interface(--iface) needs to be added if virtual box used to create VMs.
```yaml
      containers:
      - args:
        - --ip-masq
        - --kube-subnet-mgr
        - --iface eth1
```
Apply flannel configuration in cluster

```bash
kubectl apply -f kube-flannel.yml
```

Veirfy all pods running without any issues.

```bash
kubectl get pods --all-namespaces
```  

Join worker nodes to cluster
---------------------------------
Copy the below command from the output in notepad and execute in all worker nodes and make sure they joined cluster.
```bash
kubeadm join <control-plane-host>:<control-plane-port> --token <token> --discovery-token-ca-cert-hash sha256:<hash>
```

verify all nodes are ready and all pods up and running.
```bash
kubectl get nodes
kubectl get pods --all-namespaces
```

Verify cluster operation with an example
------------------------------------------------


Take an example application and deploy it in the cluster to make sure the pods are running without any issues.
