## Running as services and pods

![[Pasted image 20231123193531.png]]

==voting-app-pod.yaml==
```
apiVersion: v1
kind: Pod
metadata:
	name: voting-app-pod
	labels:
		name: voting-app-pod
		app: demo-voting-app
spec:
	containers:
		- name: voting-app
		  image: kodekloud/examplevotingapp_vote:v1
		  ports:
			  - containerPort: 80
	
```

==result-app-pod.yaml==
```

apiVersion: v1
kind: Pod
metadata:
	name: result-app-pod
	labels:
		name: result-app-pod
		app: demo-voting-app
spec:
	containers:
		- name: result-app
		  image: kodekloud/examplevotingapp_result:v1
		  ports:
			  - containerPort: 80
	
```

==redis-pod.yaml==

```
apiVersion: v1
kind: Pod
metadata:
	name: redis-pod
	labels:
		name: redis-pod
		app: demo-voting-app
spec:
	containers:
		- name: redis
		  image: redis
		  ports:
			  - containerPort: 6379
	
```
  
==postgres-pod.yaml==

```
apiVersion: v1
kind: Pod
metadata:
	name: postgres-pod
	labels:
		name: postgres-pod
		app: demo-voting-app
spec:
	containers:
		- name: postgres
		  image: postgres
		  ports:
			  - containerPort: 5432
		  env:
			  - name: POSTGRES_USER
			    value: "postgres"
			  - name : POSTGRES_PASSWORD
			    value : "postgres"
	
```

==workerapp-pod.yaml==

```
apiVersion: v1
kind: Pod
metadata:
	name: worker-app-pod
	labels:
		name: woker-app-pod
		app: demo-voting-app
spec:
	containers:
		- name: worker-app
		  image: kodekloud/examplevotingapp_worker:v1
	
```

==redis-service.yaml==

```
apiVersion: v1
kind: Service
metadata:
	name: redis
	labels:
		name: redis-service
		app: demo-voting-app
spec:
	ports:
		- port: 6379
		  targetPort: 6379
	selector:
		name: redis-pod
		app: demo-voting-app
```

==postgres-service.yml==

```
apiVersion: v1
kind: Service
metadata:
	name: db
	labels:
		name: postgres-service
		app: demo-voting-app
	
spec:
	ports:
		- port: 5432
		  targetPort: 5432
	selector:
		name: postgres-pod
		app: demo-voting-app
```

==voting-app-service.yml==

```
apiVersion: v1
kind: Service
metadata:
	name: voting-service
	labels:
		name: voting-service
		app: demo-voting-app
	
spec:
	type: NodePort
	ports:
		- port: 80
		  targetPort: 80
		  nodePort: 30004
	selector:
		name: voting-app-pod
		app: demo-voting-app
```

==result-app-service.yaml==
```
apiVersion: v1
kind: Service
metadata:
	name: result-service
	labels:
		name: result-service
		app: demo-voting-app
	
spec:
	type: NodePort
	ports:
		- port: 80
		  targetPort: 80
	      nodePort: 30005
	selector:
		name: result-app-pod
		app: demo-voting-app

```

`kubectl create -f voting-app-pod.yml`
`kubectl create -f voting-app-service.yml`

`kubectl get pods,svc`

get url of service
`minikube service voting-service --url`

`kubectl create -f redis-pod.yml`
`kubectl create -f redis-service.yml`

`kubectl create -f postgres-pod.yml`
`kubectl create -f postgres-service.yml`

`kubectl create -f worker-pod.yml`

`kubectl create -f result-app-pod.yml`
`kubectl create -f redis-app-service.yml`

## Using deployments

![[Pasted image 20231123193656.png]]

==voting-app-deploy.yml==

```
apiVerson: v1
kind: Deployment
metadata:
	name: voting-app-deploy
	labels:
		name: voting-app-deploy
		app: demo-voting-app
spec:
	replicas: 1
	selector:
		matchLabels:
			name: voting-app-pod
			app: demo-voting-app

	template:
		metadata:
			name: voting-app-pod
			labels:
				name: voting-app-pod
				app: demo-voting-app
		spec:
			containers:
				- name: voting-app
				  image: kodekloud/examplevotingapp_vote:v1
				  ports:
					- containerPort: 80
		
```

==redis-deploy.yml==

```
apiVerson: v1
kind: Deployment
metadata:
	name: redis-deploy
	labels:
		name: redis-deploy
		app: demo-voting-app
spec:
	replicas: 1
	selector:
		matchLabels:
			name: redis-pod
			app: demo-voting-app

	template:
		metadata:
			name: voting-app-pod
			labels:
				name: voting-app-pod
				app: demo-voting-app
		spec:
			containers:
				- name: voting-app
				  image: kodekloud/examplevotingapp_vote:v1
				  ports:
					  - containerPort: 80
```

==postgres-deploy.yml==

```

apiVerson: v1
kind: Deployment
metadata:
	name: postgres-deploy
	labels:
		name: postgres-pod
		app: demo-voting-app
spec:
	replicas: 1
	selector:
		matchLabels:
			name: postgres-pod
			app: demo-voting-app

	template:
		metadata:
			name: postgres-pod
			labels:
				name: postgres-pod
				app: demo-voting-app
		spec:
			containers:
				- name: postgres
				  image: postgres
				  ports:
					  - containerPort: 5432
				  env:
					  - name: POSTGRES_USER
					    value: "postgres"
					  - name : POSTGRES_PASSWORD
					    value : "postgres"
	
```

==worker-app-deploy.yml==

```

apiVerson: v1
kind: Deployment
metadata:
	name: worker-deploy
	labels:
		name: worker-pod
		app: demo-voting-app
spec:
	replicas: 1
	selector:
		matchLabels:
			name: worker-pod
			app: demo-voting-app

	template:
		metadata:
			name: worker-app-pod
			labels:
				name: woker-app-pod
				app: demo-voting-app
		spec:
			containers:
				- name: worker-app
				  image: kodekloud/examplevotingapp_worker:v1
```

==result-app-deploy.yml==

```
apiVerson: v1
kind: Deployment
metadata:
	name: result-app-deploy
	labels:
		name: result-app-deploy
		app: demo-voting-app
spec:
	replicas: 1
	selector:
		matchLabels:
			name: result-app-pod
			app: demo-voting-app

	template:
		metadata:
			name: result-app-pod
			labels:
				name: result-app-pod
				app: demo-voting-app
		spec:
			containers:
				- name: result-app
				  image: kodekloud/examplevotingapp_result:v1
				  ports:
					  - containerPort: 80
```

clean and stopped everything

`kubectl create -f voting-app-deploy.yml`
`kubectl create -f voting-app-service.yml`

`kubectl get deployments`

`kubectl create -f redis-deploy.yml`
`kubectl create -f redis-service.yml`

`kubectl create -f postgres-deploy.yml`
`kubectl create -f postgres-service.yml`

`kubectl create -f worker-app-deploy.yml`

`kubectl create -f result-deploy.yml`
`kubectl create -f result-service.yml`

`minikube service voting-service --url`
`minikube service result-service --url`

scale deployment
`kubectl scale deployment voting-app-deploy --replicas=3`

`kubectl get deployments voting-app-deploy`
