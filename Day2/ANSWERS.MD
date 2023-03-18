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
---
### 5- Do you see any taints on master? No
```bash
![k8sss](https://user-images.githubusercontent.com/116673091/226109051-634a3d7a-7a43-406f-83c2-612be1d9e374.png)
```
---
### 6- Apply a label color=blue to the master node
```bash
![dasdasda](https://user-images.githubusercontent.com/116673091/226109280-2652fec0-e8af-44e3-8524-19834ea0e58b.png)
```
---
### 7- Create a new deployment named blue with the nginx image and 3 replicas Set Node Affinity to the deployment to place the pods on master only NodeAffinity: requiredDuringSchedulingIgnoredDuringExecution Key: color values: blue
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
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
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: color
                operator: In
                values:
                - blue
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```
```bash 
kubectl get pods --field-selector spec.nodeName=controlplane
```
-screenshot ![jkkjkjk](https://user-images.githubusercontent.com/116673091/226111324-23397469-26cb-4bd1-8ce9-6a85e4dff859.png)

---
### 8- How many DaemonSets are created in the cluster in all namespaces?
```bash
controlplane $ kubectl get daemonsets
No resources found in default namespace.
```
----
### 9- what DaemonSets exist on the kube-system namespace?
```bash
controlplane $ kubectl get daemonsets -n kube-system
NAME         DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
canal        2         2         2       2            2           kubernetes.io/os=linux   23d
kube-proxy   2         2         2       2            2           kubernetes.io/os=linux   23d
```
---
### 10- What is the image used by the POD deployed by the kube-proxy DaemonSet
```bash
controlplane $ k describe daemonsets kube-proxy -n kube-system | grep image
    Image:      registry.k8s.io/kube-proxy:v1.26.1
```
---
### 11- Deploy a DaemonSet for FluentD Logging. Use the given specifications. Name: elasticsearch Namespace: kube-system Image: k8s.gcr.io/fluentd-elasticsearch:1.20
```bash 
controlplane $ kubectl get daemonsets -n kube-system
NAME            DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
canal           2         2         2       2            2           kubernetes.io/os=linux   23d
elasticsearch   2         2         2       1            2           kubernetes.io/os=linux   5m11s
kube-proxy      2         2         2       2            2           kubernetes.io/os=linux   23d
```
