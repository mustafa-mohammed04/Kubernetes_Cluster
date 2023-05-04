# Kubernetes_Cluster
## deployment-file.yaml
``` bash
vim anubisdeploy.yaml
```
``` yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: anubis-deployment
spec:
  replicas: 4
  selector:
    matchLabels:
      app: anubisapp
  template:
    metadata:
      labels:
        app: anubisapp
    spec:
      containers:
      - name: anubis-container
        image: nginx:latest
        ports:
        - containerPort: 8080
      restartPolicy: Always

```

## To apply
``` bash
kubectl apply -f anubisdeploy.yaml
```

## To Display
``` bash
kubectl get deployments
kubectl get deployment anubis-deployment
kubectl get pods       >> replicas: 4 >>> pods: 4   anubis-deploymen

```
## Scale in Deployment
``` bash
kubectl scale deployment anubis-deployment --replicas=2
kubectl get pods     >> 2 not 4
kubectl get deployments   >>  UP-TO-DATE : 2
```

## Roolout in deployment

``` bash
kubectl rollout status deployment nginx-deployment            >>  deployment "nginx-deployment" successfully rolled out OR
kubectl rollout status deployment/nginx-deployment
kubectl rollout undo deployment/nginx-deploymen          >>  make rollback
kubectl rollout history deployment/nginx-deployment          >> OR
kubectl rollout history deployment nginx-deployment
REVISION  CHANGE-CAUSE
1         <none>
```

## Describe deployment
``` bash
kubectl describe deployment/nginx-deployment        >> OR
kubectl describe deployment nginx-deployment
```

## Expose ports
``` bash
kubectl expose deployment anubis-deployment --port=8080 --target-port=80        >>  service/anubis-deployment exposed

```
## Delete Deployment
 ``` bash
 kubectl delete deployment anubis-deployment
 
 ```
 ## To Create deployment Using create Subcommand
 ``` bash
 kubectl create deployment iti43deploy --image=alpine:latest --replicas=3 > iti43deploy.yaml
 kubectl get deployments iti43deploy
 ```
