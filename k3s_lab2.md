# K8S_k3s
## Flow 
![Lab2](https://user-images.githubusercontent.com/128603198/234144998-cbec0815-7fdf-4cdb-8ead-11b945dde2ac.png)

#### In Control-plane
## Install curl and k3s

``` bash
apt install curl -y
curl -sfL https://get.k3s.io | sh -

```
## To know pods and nodes

``` bash
kubectl get nodes
kubectl get pods -A

```

## The value to use for K3S_TOKEN and using ip of Conrtol-plane

``` bash
ifconfig ens33  >> 192.168.157.233
cat  /var/lib/rancher/k3s/server/node-token
K10b96562d86a8a63fcc5457265388aff843f32c48164bc152a1a6073f6572f178b::server:eeecb4b148898397121e038346773d03

```

#### In Worker-node to join

``` bash
curl -sfL https://get.k3s.io | K3S_URL=https://192.168.157.233:6443 K3S_TOKEN=K10b96562d86a8a63fcc5457265388aff843f32c48164bc152a1a6073f6572f178b::server:eeecb4b148898397121e038346773d03 sh -
```



#### In Master-node

## Create namespace

```  bash

    kubectl create namespace iti-43
    kubectl get namespaces
    kubectl describe namespace iti-43

```

## Edit the kubectl config file to add a new context that uses the default user with the new namespace you just created

``` bash
kubectl config set-context iti-43-context --cluster=k3s-default --user=default --namespace=iti-43
kubectl config use-context iti-43-context

```

## Create a new kubectl plugin called “kubectl hostnames” that should display the hostnames of all your nodes on the cluster

``` bash
mkdir ~/kubectl-hostnames
cd ~/kubectl-hostnames
```
## In File kubectl-hostnames.go

``` go
package main

import (
	"fmt"
	"os"
	"text/tabwriter"

	"k8s.io/cli-runtime/pkg/genericclioptions"
	"k8s.io/client-go/kubernetes"
	"k8s.io/client-go/tools/clientcmd"
)

func main() {
	// Create a new tabwriter to format the output nicely
	w := tabwriter.NewWriter(os.Stdout, 0, 0, 2, ' ', 0)

	// Get the kubeconfig file path from the KUBECONFIG environment variable
	kubeconfig := os.Getenv("KUBECONFIG")
	if kubeconfig == "" {
		kubeconfig = os.Getenv("HOME") + "/.kube/config"
	}

	// Load the kubeconfig file
	config, err := clientcmd.BuildConfigFromFlags("", kubeconfig)
	if err != nil {
		fmt.Fprintf(os.Stderr, "Error loading kubeconfig file: %v\n", err)
		os.Exit(1)
	}

	// Create a new Kubernetes clientset
	clientset, err := kubernetes.NewForConfig(config)
	if err != nil {
		fmt.Fprintf(os.Stderr, "Error creating Kubernetes clientset: %v\n", err)
		os.Exit(1)
	}

	// Get the list of nodes
	nodes, err := clientset.CoreV1().Nodes().List(genericclioptions.ListOptions{})
	if err != nil {
		fmt.Fprintf(os.Stderr, "Error getting nodes: %v\n", err)
		os.Exit(1)
	}

	// Print the list of node hostnames
	fmt.Fprintf(w, "NAME\tHOSTNAME\n")
	for _, node := range nodes.Items {
		fmt.Fprintf(w, "%s\t%s\n", node.GetName(), node.Status.Addresses[0].Address)
	}
	w.Flush()
}

```

## Build the plugin by running the following command

``` bash
go build -o kubectl-hostnames
```

## Move the kubectl-hostnames >> /usr/local/bin
``` bash
mv kubectl-hostnames /usr/local/bin/
```

## Binary executable or permission
``` bash
chmod +x /usr/local/bin/kubectl-hostnames
```

## Test Plugin
``` bash
kubectl hostnames

```

#### Create a deployment YAML file that has 3 replicas of image “nginx:alpine

## create a filename.yaml
``` bash
vim nginx-deployment.yaml
```
## Inside to this file "nginx-deployment.yaml"

``` yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
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
        image: nginx:alpine
        ports:
        - containerPort: 80


```

## Apply the YAML file to the cluster

``` bash
kubectl apply -f nginx-deployment.yaml

```

## Verify Deployment has been created and the pods are running

``` bash
kubectl get deployments,pods

```








