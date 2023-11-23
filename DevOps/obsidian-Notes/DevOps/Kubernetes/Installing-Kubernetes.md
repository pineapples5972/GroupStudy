- [ ] Install kubectl utillity
- [ ] Install a hyperviser such as kvm, quemu or virtualbox
- [ ] Install minicube

start minikube
`minikube start --driver=virtualbox`

check status
`minikube status`

check if its working
`kubctl get nodes`

Try creating example deployment
`kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.10`

expose the service
`kubectl expose deployment hello-minikube --type=NodePort --port=8080`

get url of the service
`minikube service hello-minikube --url`

check on browser if its working

clean up
`kubectl delete deployment hello-minikube`
`kubectl delete service hello-minikube`


Reference: 
1. https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/
2. https://www.youtube.com/watch?v=XuSQU5Grv1g
3. https://k8s-docs.netlify.app/en/docs/tasks/tools/install-minikube/

