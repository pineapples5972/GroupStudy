Kubernetes can use to deploy more than one instance of containers using its cli.


![[docker-cluster.excalidraw]]
https://www.youtube.com/watch?v=XuSQU5Grv1g

| Kind | Version |
| --- | --- |
| Pod | v1 |
| Service | v1 |
| ReplicaSet | apps/v1 |
| Deployment | apps/v1 | 

for more `kubectl api-resources`

==pod-definition.yml==
``` 
apiVersion: v1
kind: Pod
metadata: 
	name: myapp-pod
	labels:
		app: myapp
		

spec:
```

==pod.yml==
```
apiVersion: v1
kind: Pod
metadata: 
	name: nginx
	labels:
		app: nginx
		tier: frontend
spec:
	containers:
	- name: nginx
	  image: nginx
```

`kubectl create or kubectl apply -f pod.yml`

`kubectl describe pod nginx`

pod run on one node at a time

### ReplicaSets
==ReplicaSet ensures that a specified number of pod replicas are running at any given time.==

list replicasets - `kubectl get replicaset` or `kubectl get rs`
describe replicaset - `kubectl describe replicaset`
change settings of replicaset with yml file -
`kubectl replace -f replicaset-definition.yaml`

scale replicas to 6
`kubectl scale --replicas=6 -f replicaset-definition.yml`

`kubectl scale --replicas=6 replicaset myapp-replicaset`

==replicaset-definition.yml==
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
	name: myapp-replicaset
	labels:
		app: myapp
		type: front-end
spec:
	template:
		metadata:
			name: myapp-pod
			labels:
				app: myapp
				type: front-end
		spec:
			containers:
			- name: nginx-container
			  image: nginx
	replicas: 3
	selector:
		matchLabels:
			type: front-end
```
apply replicaset - `kubectl create -f replicaset-definition.yml`
