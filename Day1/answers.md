# kubernets lab
## day 1
### 1. Create a pod with the name redis and with the image redis.
![Screenshot_20230203_024110](https://user-images.githubusercontent.com/116673091/216484321-3d677ec9-1b94-47cb-816c-a36ef102eee5.png)
---
### 2. Create a pod with the name nginx and with the image “nginx123” Use a pod-definition YAML file.
- ![Screenshot_20230203_025348](https://user-images.githubusercontent.com/116673091/216484945-832276a3-d784-4c98-8d7b-4ec5fe8407a5.png)
- pod created
![Screenshot_20230203_025654](https://user-images.githubusercontent.com/116673091/216485458-48f32409-6775-45c5-ad7c-3b2f4d87a47a.png)
---
### 3. What is the nginx pod status?
- failing to pull image
---
### 4. Change the nginx pod image to “nginx” check the status again
- running ![Screenshot_20230203_030042](https://user-images.githubusercontent.com/116673091/216485960-0ed048ec-9eb0-49bc-b423-d21d5fd34a85.png)
---
### 5. How many ReplicaSets exist on the system? zero
-![Screenshot_20230203_030207](https://user-images.githubusercontent.com/116673091/216486078-693593bc-90d8-4ad7-a641-388b40810898.png)
---
### 6.  create a ReplicaSet with name= replica-set-1 image= busybox replicas= 3
- yaml file![Screenshot_20230203_031609](https://user-images.githubusercontent.com/116673091/216487747-1ff1d895-84a9-4989-a7e6-75595142be7c.png)
---
### 7. Scale the ReplicaSet replica-set-1 to 5 PODs
```bash
kubectl scale --replicas=5 replicaset replica-set-1
```
---
### 8. How many PODs are READY in the replica-set-1?
- 5 pods are ready 
---
### 9. Delete any one of the 5 PODs then check How many PODs exist now? 
- five pods
## Why are there still 5 PODs, even after you deleted one? 
- because replicaset is scaling up
---
### 10. How many Deployments and ReplicaSets exist on the system?
- Deployments =0 ReplicaSets =1 (replica-set-1)
---
### 11. create a Deployment with name= deployment-1 image= busybox replicas= 3
- yaml file ![555555555555555555555555555555555555555555555555555555](https://user-images.githubusercontent.com/116673091/216492271-5090f8a3-7bca-4aa8-bc1d-b44f80e15f96.png)
---
### 12. How many Deployments and ReplicaSets exist on the system now?
- deployments = 1 replicasets =1
---
### 13. How many pods are ready with the deployment-1?
- 3 pods are ready 
---
### 14. Update deployment-1 image to nginx then check the ready pods again
```bash
set image deployment/deployment-1 busybox=nginx~
```
---
### 15. Run kubectl describe deployment deployment-1 and check events 
# What is the deployment strategy used to upgrade the deployment-1?
- RollingUpdate
---
### 16- Rollback the deployment-1
```kubectl rollout undo deployment/deployment-1```
## What is the used image with the deployment-1?
- busybox image
---
### 17.  Create a deployment using nginx image with latest tag only and remember to mention tag i.e nginx:latest and name it as nginx-deployment. App labels should be app: nginx-app and type: front-end. The container should be named as nginx-container; also make sure replica counts are 3.
``` yaml 
apiVersion: apps/v1

kind: Deployment

metadata:

  name: nginx-deployment

  labels:

    app: nginx-app
    type: fron-end

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

      - name: nginx-container

        image: nginx:latest

        ports:

        - containerPort: 80
```

