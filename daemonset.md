# Kubernetes_Cluster

## DaemonSet
``` bash
vim ds.yaml
```
``` yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: my-daemonset
spec:
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      tolerations:
      - key: "node-role.kubernetes.io/master"
        operator: "Exists"
        effect: "NoSchedule"
      containers:
      - name: my-container
        image: nginx:latest
      restartPolicy: Always

```

## To apply
``` bash
kubectl apply -f ds.yaml
```
## To Display
``` bash
kubectl get ds         >> my-daemonset
kubectl get pods        >> my-daemonset

```
## Logs in ds
``` bash
kubectl logs daemonset/my-daemonset
```

## Delete ds
``` bash 
kubectl delete daemonset my-daemonset

```
