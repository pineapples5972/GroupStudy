Kubernetes can use to deploy more than one instance of containers using its cli.


![[docker-cluster.excalidraw]]
https://www.youtube.com/watch?v=XuSQU5Grv1g

| Kind | Version |
| --- | --- |
| Pod | v1 |
| Service | v1 |
| ReplicaSet | apps/v1 |
| Deployment | apps/v1 | 

pod-definition.yml
``` 
apiVersion: v1
kind: Pod
metadata: 
	name: myapp-pod
	labels:
		app: myapp
		

spec:
```

pod.yml
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

kubectl describe pod nginx




