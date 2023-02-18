### 1- How many Namespaces exist on the system? 4 name spaces
```bash
kubectl get namespaces
```
- screenshot ![22](https://user-images.githubusercontent.com/116673091/219855914-b309736d-c1c6-4739-a537-8dfd6a73e037.png)
---
### 2- How many pods exist in the kube-system namespace? 9 pods
```bash 
kubectl get pods --namespace=kube-system
```
- screenshot ![Screenshot from 2023-02-18 12-42-01](https://user-images.githubusercontent.com/116673091/219856048-a3b86b7d-ad7f-4b26-a695-809670f3296a.png)
---
### 3- Create a deployment with Name: beta Image: redis Replicas: 2 Namespace: finance Resources Requests: CPU: .5 vcpu Mem: 1G Resources Limits: CPU: 1 vcpu Mem: 2G
- first i create finance namespace 
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: finance
```
- deployment file
```yaml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: beta
  labels:
    app: redis
    namespace: finance
spec:
  replicas: 2
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis
        image: redis:5.0.4
        resources:
          requests:
            memory: "1Gi"
            cpu: "500m"
          limits:
            memory: "2Gif"
            cpu: "1"
```
### 4- How many Nodes exist on the system? 1

