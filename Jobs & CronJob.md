# Kubernetes_Cluster
## Job  "Something / task works and then it gets killed > 0/1 completed"
## To Create Job and redirect in yaml file
``` bash
kubectl create job --dry-run=client -o yaml anubisjob --image=alpine:latest > anub_job.yaml
vim anub_job.yaml
```
``` yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: anubisjob
  namespace: default
spec:
  template:
    spec:
      containers:
      - image: alpine:latest
        name: anubisjob
      restartPolicy: Never
  backoffLimit: 4   

```
## To apply
``` bash
kubectl apply -f anub_job.yaml
```

## Describe Job and logs of Job and delete job
``` bash
kubectl describe job anubisjob
kubectl get pods 
kubectl logs anubisjob-5s2jk
kubectl delete job anubisjob 

```

## CronJob  "Schedule Tasks for a specific period OR always"
## In CronJob yaml file
``` bash
vim cronjob.yaml
```
``` yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: my-cronjob
  namespace: default
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: my-container
            image: alpine:latest
            command:
            - /bin/sh
            - -c
            - date; echo "Hello, Mustafa!"
          restartPolicy: OnFailure
```
## To apply
``` bash
kubectl apply -f cronjob.yaml
```
## To Display Cronjobs
``` bash
kubectl get cronjobs    
``` 
``` bash
NAME         SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE
my-cronjob   */1 * * * *   False     1        4s              9m49s

```
## Describe Cronjob
``` bash
kubectl describe cronjob my-cronjob
```
## Dispaly Pods
``` bash
kubectl get pods           >> my-cronjob-28053550-g2x86           0/1     Completed          0                3s
```
##  Delete CronJob
``` bash
kubectl delete cronjob my-cronjob
```
