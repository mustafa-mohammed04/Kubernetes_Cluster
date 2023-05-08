# Kubernetes_Cluster
## StorageClass : allow you make pvc without making pv by default make pv directly

## StoargeClass
``` bash
kubectl get storageclass         >>  PROVISIONER  rancher.io/local-path
NAME                   PROVISIONER                RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
local-path (default)   rancher.io/local-path      Delete          WaitForFirstConsumer   false                  16h
```

## Make StorageClass
``` bash
vim storageclass.yaml
```
``` yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: nfs-storage-class
provisioner: example.com/external-nfs
parameters:
  server: nfs-server.example.com
  path: /share
  readOnly: "false"
```
## apply storageclass.yaml and display storageclass
``` bash
kubectl apply -f storageclass.yaml
kubectl get storageclass

NAME                   PROVISIONER                RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
local-path (default)   rancher.io/local-path      Delete          WaitForFirstConsumer   false                  16h
nfs-storage-class      example.com/external-nfs   Delete          Immediate              false                  5m54s
```

## Make pvc and attach sc
``` bash
vim pvc.yaml
```
``` yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 1Gi
  storageClassName: nfs-storage-class

```
## apply pvc and display sc, pvc and pv
``` bash
kubectl apply -f pvc.yaml
kubectl get storageclass
kubectl get pvc
kubectl get pv

```
