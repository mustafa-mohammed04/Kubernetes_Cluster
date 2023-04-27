# Kubernetes_Cluster

## Flow 
![Flow 1](https://user-images.githubusercontent.com/128603198/234753534-2005703a-4dd8-4658-9f82-9e02be8f5c44.png)
![Flow 2](https://user-images.githubusercontent.com/128603198/234753535-4320c1a2-6cdf-4ee7-ae61-63be14a5fa5e.png)

## Create a PersistentVolume named cool-volume.yaml
![1](https://user-images.githubusercontent.com/128603198/234753724-2c4bf415-a1df-442d-b381-e2c4dd3c3e31.png)

## to create
``` bash
kubectl create -f /tmp/cool-volume.yaml
```

## Create a PersistentVolumeClaim named my-claim that requests 
a volume of size 100Mi
![2](https://user-images.githubusercontent.com/128603198/234753971-258e9482-9eea-4496-80b8-c51b8e953b76.png)

## To create PersistentVolumeClaim.yaml

``` bash
kubectl create -f my-claim.yaml
```


## Create a pod named pvc-user in namespace default that  mounts your PVC my-claim under /mnt/share/my-pvc (nginx image)
      
    
![3](https://user-images.githubusercontent.com/128603198/234754211-b5da4818-a730-435a-8646-ac5ca32f775f.png)

## To create 

``` bash
kubectl create -f pvc-user.yaml
```

## Create a file on your system on /opt/cm.yaml with content:

``` yaml
apiVersion: v1
data:
 tree: birke
 level: "3"
 department: park
kind: ConfigMap
metadata:
 name: birke
 ```
 ![4](https://user-images.githubusercontent.com/128603198/234754448-614c2238-f399-4bbc-8d52-1432991cbde6.png)


## Create a ConfigMap named trauerweide with content  tree=trauerweide

![5](https://user-images.githubusercontent.com/128603198/234754570-b480d51e-33b5-4be1-9e3d-2775f703cef8.png)

## To Create 
``` bash
kubectl create -f  trauerweide-configmap.yaml
```
 
## Create a Pod named pod1 of image nginx:alpine and make key  tree of ConfigMap trauerweide available as environment variable TREE1 also Mount all keys of ConfigMap birke as   volume. The files should be available under /etc/birke/* 
      
        
 
 ``` bash
 vim pod1.yaml
 
 ```
![6](https://user-images.githubusercontent.com/128603198/234754999-2e99a43c-dca5-4384-9a9b-974653d6d0c3.png)

## To create 
 ``` bash
 kubectl create -f pod1.yaml
 ```
    
    
