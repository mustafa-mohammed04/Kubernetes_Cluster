# Kubernetes_Cluster
## hostPath Volume

## make dirctory and change dir
``` bash
mkdir volume_sql
cd volume_sql
```

## Create Deployment
``` bash
kubectl create deployment mysql --image=msql --replicas=1 --dry-run=client -o yaml > mysql.yaml
vim mysql.yaml
```
``` yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: mysql
  name: mysql
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: mysql
    spec:
      volumes:
      - name: mysql-data
        hostPath:
          path: /data/mysql
          type: DirectoryOrCreate
      containers:
      - image: mysql
        name: mysql
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: "anubis"
        volumeMounts:
        - name: mysql-data
          mountPath: /var/lib/mysql

status: {}

```

## apply yaml file and display pods and deployments
``` bash
kubectl apply -f mysql.yaml
kubectl get pods -A           >>  mysql-85d4f68c5c-cwhrb
kubectl get deployments
ls /data/mysql
```
## Inside pod
``` bash
kubectl exec -it  mysql-85d4f68c5c-cwhrb bash
mysql -uroot -panubis                 >> user: root , valuepassword: anubis     in mysql.yaml
create database iti43;
show databases;     >> iti43
```

## Delete pod of mysql 
``` bash
kubectl delete pod mysql-85d4f68c5c-cwhrb           >> establish another pod with different name "mysql-85d4f68c5c-xlrxh" AND has same database that created iti43 database 
kubectl get pods -A
kubectl exec -it mysql-85d4f68c5c-xlrxh bash
mysql -uroot -panubis
show databases;          >> iti43
```
