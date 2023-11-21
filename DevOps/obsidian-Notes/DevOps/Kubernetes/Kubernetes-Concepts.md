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
ReplicaSets creates replicas of defined number of pods and make sure they always running and recreate them whenever some failed or deleted.

==ReplicaSet ensures that a specified number of pod replicas are running at any given time.==

To create replicasets we use ==replicaset-definition.yml== file and defined the kind and number of replicas we want.
like in this example:

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

and we apply with this command:
apply replicaset - `kubectl create -f replicaset-definition.yml`

- list replicasets - `kubectl get replicaset` or `kubectl get rs`
- describe replicaset - `kubectl describe replicaset`
- delete replicasets - `kubectl delete replicaset replicaset-1`

To make changes just edit the yaml file and apply modified changes with replace command:
`kubectl replace -f replicaset-definition.yaml`

If you wish to change number replicas can be done via scale command:

scale replicas to 6:
`kubectl scale --replicas=6 -f replicaset-definition.yml`

the recommended way is as above by modifying the yaml file but can also input name of replicaset like below but not recommended:

`kubectl scale --replicas=6 replicaset myapp-replicaset`
