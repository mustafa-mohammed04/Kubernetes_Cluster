# Kubernetes_Cluster
## Networking Model 
``` bash
every pod in cluster get its own unique cluster wide ip-address
cluster will have 2 main cidr
1. cluster-cidr "pod-cidr"       2. service-cidr

10.244.0.0/16   flannel in kubeadm      

10.42.0.0/16 cluster-cidr                                 
10.43.0.0/16 service-cidr
```
![cluster-cidr](https://user-images.githubusercontent.com/128603198/236455097-1c71bdb0-939b-43bf-903e-8b4394346d53.png)

## Flannel "Operates a UDP protocol"
![Flannel](https://user-images.githubusercontent.com/128603198/236455165-9154f53d-cbb4-4db6-aa57-12a4aae97fb5.png)

## Put cluster-cidr
``` bash
mkdir -p /etc/rancher/k3s/
vim /etc/rancher/k3s/config.yaml
```
``` yaml
cluster-cidr: 10.42.0.0
```
## Put service-cidr
``` bash
mkdir -p /etc/rancher/k3s/
vim /etc/rancher/k3s/config.yaml
```
``` yaml
kube-apiserver:
  service-cluster-ip-range: 10.43.0.0

```
``` bash
systemctl restart k3s.service
```
##
``` bash
kuebctl get pods -o wide
curl http://10.42.0.9       >> nginx
curl http://10.42.1.3       >> nginx
curl http://10.42.1.4       >> nginx
```


## Service
``` bash
method for exposing a networking app that is runing as one / more pods in your cluster
```
## Why make service ? 
``` bash
 because the pod isn't immutable object when crash to pod 
  call pod through service   >> kube proxy : watch api server "when make service, the service collect all ips"
```
![services](https://user-images.githubusercontent.com/128603198/236468855-6c72fcec-7bee-4aba-af79-7b1eab352e05.png)

![module_04_labels](https://user-images.githubusercontent.com/128603198/236468904-229779f0-60db-4713-8022-f9c14b10bc4e.svg)

<img width="933" alt="service-annotated" src="https://user-images.githubusercontent.com/128603198/236468913-b8b812ea-0d20-4336-a38f-3281416c6f59.png">

<img width="1155" alt="kube-proxy-service-discovery" src="https://user-images.githubusercontent.com/128603198/236468933-b99dc3d8-1d29-474a-a295-3d5cfd77329b.png">

## To Expose
``` bash
kubectl expose deployment anubis-deployment --port 8080 --target-port 80
kubectl get services
kubectl get services -o wide
```
## To Verify
``` bash
curl http://10.43.177.154:8080         >>  10.43.177.154 ip of anubis-deployment service
```
## Describe, edit and delete service
``` bash
kubectl describe deployment anubis-deployment
kubectl edit deployment anubis-deployment
kubectl delete deployment anubis-deployment
```
## To Inside this service  "Info"
 ``` bash
 kubectl get services anubis-deployment -o yaml
 ```

## To make port-forward
``` bash
kubectl port-forward anubis-deployment-5f987d69f4-47qxr 8080:80 >> 
anubis-deployment-5f987d69f4-47qxr is one of anubis-deployment
8080: local-port
80: pod-port
```
## To display services on host level
``` bash
iptables --list-rules -t nat | grep anubis-deployment
```

## To Create Service Nodeport

``` bash
kubectl create service nodeport nginx-np --tcp "8080:80" --dry-run=client -o yaml > service-np.yaml
```
``` yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: nginx-nodeport
  name: nginx-nodeport
spec:
  ports:
  - name: 8080-80
    nodePort: 31731
    port: 8080
    protocol: TCP
    targetPort: 80
  selector:
    app: nginx-nodeport
  type: NodePort
status:
  loadBalancer: {}

```
``` bash
kubectl apply -f service-np.yaml
kubectl get services 
curl icanhazip.com        > ip 54.218.72.53
curl curl http://54.218.72.53:31612        >> 31612 ex port
```

