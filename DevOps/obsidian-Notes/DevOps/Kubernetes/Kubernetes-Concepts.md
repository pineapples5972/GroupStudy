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

### Deployments
We normally deploy containers or pods individually or with replicasets but deployments can provide a more powerful and flexible way to manage pods, __managing their lifecycles and rolling or rollback updates__ and they can help to reduce downtime and improve  the reliability of your applications.

Deployments should be used in the following cases:
- You need to perform rolling updates or rollbacks.
- You need to perform health checks on pods.
- You want to use a more declarative and automated way to manage pods.

we can use yaml file to create deployments:
==deployment-definition.yml==
```
apiVersion: apps/v1
kind: Deployment
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

and apply with this command:
`kubectl create -f deployment-definition.yml`

or more imperative way would be to like this: 

for example the following command creates a deployment named `my-deployment` that runs three replicas of the `nginx:latest` container image: 

`kubectl create deployment my-deployment --image=nginx:latest --replicas=3`

This command will create three pods, each running the `nginx` container image. The deployment will also manage the lifecycle of these pods, ensuring that there are always three pods running, even if one or more of the pods fail.


