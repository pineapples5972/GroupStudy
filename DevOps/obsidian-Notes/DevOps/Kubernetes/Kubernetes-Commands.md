#### Pod commands 
- list nodes - `kubectl get nodes`
- see version - `kubectl version`
- run a pod - `kubectl run nginx --image=nginx`
- run with generating yaml file - `kubectl run redis --image=redis --dry-run=client -o yaml > redis.yaml`
- list pods - `kubectl get pods` more details `-o wide` option 
- edit pods - `kubectl edit pod podname`
- create resources(same as create)- `kubectl apply -f pod.yaml`
- describe pod same as docker inspect - `kubectl describe pod nginx`
- delete pod - `kubectl delete pod nginx`
- apply definition - `kubectl apply -f pod.yml`  

#### ReplicaSet commands
- list replicasets - `kubectl get replicaset` or `kubectl get rs`
- delete replicasets - `kubectl delete replicaset replicaset-1`
- create replicaset from yml file - `kubectl create -f replicaset-definition.yml`
- describe replicaset - `kubectl describe replicaset`
- update the settings of replicaset by editing the yaml file - `kubectl replace -f replicaset-definition.yaml`
- scale replicas to 6 - `kubectl scale --replicas=6 -f replicaset-definition.yml` or can do with name `kubectl scale --replicas=6 replicaset myapp-replicaset` but recommended method is with yaml file

#### Deployment Commands

- create deployment - `kubectl create deployment my-deployment --image=nginx:latest`
- create deployment with yaml file - `kubectl create -f deployment-definition.yml`
- list deployment - `kubectl get deployments`
- scale deployment - `kubectl scale deployment my-deployment --replicas=5`
- get information of deployment - `kubectl get deployment my-deployment`
- describe deployment - `kubectl describe deployment my-deployment`
- delete a deployment - `kubectl delete deployment my-deployment`

#### Service Commands
- list a service - `kubectl get services`
- create a service - `kubectl -f service-definition.yml`
- describe a service - `kubectl describe service service-name`
- delete a service - `kubectl delete service service-name`

