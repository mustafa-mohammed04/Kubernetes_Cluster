#### Lab 1
![lab1](https://user-images.githubusercontent.com/128603198/233872053-09435617-50df-4007-845c-5f8e5569d1ba.png)

## Result of nodes and pods (status)
![3](https://user-images.githubusercontent.com/128603198/233872077-48c28fbb-31de-4ef5-80b7-77b409a5895f.png)
![4](https://user-images.githubusercontent.com/128603198/233872091-e0eac7d5-eb00-483c-ba4e-90b07db529bc.png)



#### Commands



## Install docker.io and containerd
``` bash
sudo apt install docker.io
sudo apt install containerd
```

## 


``` bash
curl -fsSL  https://packages.cloud.google.com/apt/doc/apt-key.gpg|sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/k8s.gpg
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt update
```

## Install with a custom version and hold any update
``` bash
sudo apt install wget curl vim git kubelet=1.25.8-00 kubeadm=1.25.8-00 kubectl=1.25.8-00 -y
sudo apt-mark hold kubelet kubeadm kubectl
```


## Turn off swap
``` bash
sudo swapoff -a 
```

## Configure persistent  loading of modules

``` bash
sudo tee /etc/modules-load.d/k8s.conf <<EOF
overlay
br_netfilter
EOF
```

## Load at Runtime

``` bash
sudo modprobe overlay
sudo modprobe br_netfilter
```

## Ensure sysctl params are set
``` bash
sudo tee /etc/sysctl.d/kubernetes.conf<<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF

## Reload configs
``` bash
sudo sysctl --system
```




## Configure containerd and start service

``` bash
sudo su -
mkdir -p /etc/containerd
sudo tee /etc/containerd/config.toml<<EOF
version = 2
[plugins]
  [plugins."io.containerd.grpc.v1.cri"]
   [plugins."io.containerd.grpc.v1.cri".containerd]
      [plugins."io.containerd.grpc.v1.cri".containerd.runtimes]
        [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
          runtime_type = "io.containerd.runc.v2"
          [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
            SystemdCgroup = true
EOF
```
## Restart Containerd

``` bash
sudo systemctl restart containerd
sudo systemctl enable containerd
systemctl status containerd
```
## Configure kubeadm

``` bash
sudo tee ~/kubeadm.yaml<<EOF
kind: ClusterConfiguration
apiVersion: kubeadm.k8s.io/v1beta3
kubernetesVersion: v1.25.8
controlPlaneEndpoint: 192.168.1.9
networking:
  podSubnet: "10.244.0.0/24"
---
kind: KubeletConfiguration
apiVersion: kubelet.config.k8s.io/v1beta1
cgroupDriver: systemd
EOF
```

## Start kubeadm

``` bash
kubeadm init --config kubeadm.yaml
```

## dependencies when make init "start kubeadm"

```bash
  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

## Flannel 

``` bash
kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml
```
## Nodes

``` bash
kubectl get nodes
```

## establish deployment.yaml and edit of deployment.yaml

``` bash
wget  https://gist.githubusercontent.com/galal-hussein/9dc72ecada5fda0f7e0a8d0aaa595af1/raw/032e373f2b292176aa4df2d6b842983759359998/deployment.yaml
```
## edit in deployment.yaml
``` bash
vim deployment.yaml
```
``` yaml
vim deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
      tolerations:
      - key: "node-role.kubernetes.io/control-plane"
        operator: "Exists"
        effect: "NoSchedule
```

## apply deployment.yaml

``` bash
kubectl apply -f deployment.yaml
kubectl get nodes
kubectl get pods
```




