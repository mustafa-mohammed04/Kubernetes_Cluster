# Kubernetes_Cluster
## Ingress
``` bash
way in k8s to serve of services through http/https
in Layer 7 "App Layer"
by default in k3s : traffic ingress controller like nginx webserver
able to reach to service through name-based virtual hosting
is a web sever to serve your pods in you cluster using services
nginx ingress controller : controller that waits you to make ingress OrR not
able to make a CA"Certificate" in ingress
traffic ingress controller used traffic
nginx ingress controller used nginx
apache ingress controller used apache
in ingress.yaml > depends on  ingressClassName

```
``` bash
vim ingress.yaml
```
``` yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
 name: minimal-ingress
 annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
 ingressClassName: nginx-example
 rules:
 - host: "anubis.com"
   http:
    paths:
    - path: /testpath
      pathType: Prefix
      backend:
       service:
         name: nginx
         port:
          number: 80

```

``` bash
kubectl apply -f ingress.yaml
kubectl get ingress
kubectl describe ingress minimal-ingress

```
