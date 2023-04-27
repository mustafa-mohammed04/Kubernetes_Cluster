# Kubernetes_Cluster
## Flow
![flow](https://user-images.githubusercontent.com/128603198/234750654-104516b3-e2e1-4438-b33e-9a5e5765efc8.png)



## Create NameSpace and display namespaces

``` bash
kubectl create namespace hello
```
![1](https://user-images.githubusercontent.com/128603198/234748188-09b69b5d-342b-4b5e-ba24-4a968cb9d401.png)


##  aisa with 2 replicas and image 
    “husseingalal/africa:latest”
##   europe with 2 replicas and image 
    “husseingalal/europe:latest”
    
 ``` bash
 kubectl create deployment aisa --image=husseingalal/africa:latest --replicas=2
 kubectl get pods
 kubectl create deployment aisa --image=husseingalal/europe:latest --replicas=2
 kubectl get pods
 ```
 ![2](https://user-images.githubusercontent.com/128603198/234748892-6cd274ab-7cf5-42c3-826b-ca8856133871.png)
 ![3](https://user-images.githubusercontent.com/128603198/234748912-30693ae1-60bd-4a52-bd51-a5d2c3bd6602.png)


## exposing port

``` bash

kubectl expose deployment aisa --type=ClusterIP --port=8888 --target-port=80 --name=aisa
kubectl expose deployment aisa --type=ClusterIP --port=8888 --target-port=80 --name=europe
kubectl get services
```
![4](https://user-images.githubusercontent.com/128603198/234749313-3c2c16f5-bba7-4b29-b18d-d55d1fd395a3.png)

## Create a new Ingress resource called world
``` yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: world
  namespace: world
spec:
  rules:
  - host: world.universe.mine
    http:
      paths:
      - path: /europe/
        pathType: Prefix
        backend:
          service:
            name: europe
            port:
              number: 8888
      - path: /africa/
        pathType: Prefix
        backend:
          service:
            name: aisa
            port:
              number: 8888
```
## to apply ingress.yaml
``` bash
kubectl apply -f ingress.yaml
```


## hosts file > /etc/hosts
![5](https://user-images.githubusercontent.com/128603198/234750170-f7dd5dc1-d422-4ea7-915c-836a21903343.png)


## Attach URL
``` bash
curl http://world.universe.mine/europe/
```
![6](https://user-images.githubusercontent.com/128603198/234750426-3239c95f-47d5-4a67-98a6-7baddbb11f3a.png)


              
