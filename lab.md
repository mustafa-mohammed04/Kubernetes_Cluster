# Kubernetes_Cluster
## Install k3s with attching etcd

## Create Directory 
``` bash
mkdir -p /etc/rancher/k3s
```
## create a file in this Directory
``` bash
 vim /etc/rancher/k3s/config.yaml
         cluster-init: true
```
## Install k3s
``` bash
 curl -sfL https://get.k3s.io | sh -
```

## To Display Nodes and pods
``` bash
      kubectl get nodes
      kubectl get pods -A
```

## To View Logs to know etcd is attched
``` bash
journalctl -u k3s | grep etcd
```
