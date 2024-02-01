---
tags:
  - "#DevOps"
  - "#Commands"
  - "#Docker"
---

#### Docker Commands
- Run or start a container
  `docker run nginx`
  _will pull locally installed or remote container and run it._
- ps - list containers 
  `docker ps`
- list the all containers - 
  `docker ps -a`
  _It will list exited all previously containers running_
- Stop Container 
  `docker stop silly_sammet`
  _will stop running container named silly_samet (use name or ids)_
- Rm - Remove a container
  `docker rm silly_sammer`
  _will delete stopped **container** named silly_sammer_
- Images - List Images 
  `docker images`
  _Lists local available images on the host_
- Rmi - Remove images 
  `docker rmi ngnix`
  _will delete the local images_ 
- pull - download remote images
   `docker pull nginx`
  _it will only pull images and not run em_
- append a command 
   `docker run ubuntu` 
   `docker run ubuntu sleep 5`
   _It will run second command as a process in empty container_
- exec - execute a command
   `docker exec <name/ID> cat /etc/hosts`
   _exectute a command in running container_
- run - attach mode
  `docker run kodekloud/simple-webapp`
  _command like webserver will run in foreground aka attach mode to exit from them press ctrl + c or ctrl + z to suspend em in bg_
- run - dettach mode
  `docker run -d kodekloud/simple-webapp`
  _this will run the command in dettach mode aka background mode_
- it - interactive mode
  `docker run -it centos bash`
  _This will log you into console of containerized os_
- tags - run specific version
  `docker run ubuntu:12.04
 _this will run or pull image name ubuntu of version of 12.04 tag_
 - port mapping 
  `docker run –p 80:5000 kodekloud/simple-webapp`
  _This will map port 80 of the host machine to forward traffic onto port 5000 inside container._
- volume mapping
  `docker run –v /opt/datadir:/var/lib/mysql mysql`
  _This will map default /opt/datadir directory on the host machine to /var/lib/mysql inside docker container. By using this volume mount, you ensure that the data stored in the MySQL container is persisted on the host machine, even if the container is stopped or removed._
- inspect container
   `docker inspect <name/id>`
   _This will give all configuration details of the specific container _
- seeing logs
   `docker logs <name/id>`
   _Useful for debuging issues_
- setting up enviromental variables
   `docker run -e APP_COLOR=green simple-webapp-color`
   _Useful for changing programmable variable inside a program or container_



#### Container Orchestration
when one container is becoming out of load and capacity to hold more users and data processing new instances of the container need to be spawn.
Container Orchestration contains tools that make this process easy and automated.
example: 
`docker service create --replicas=100 nodejs`

There are various docker orchestration solutions available some common are:
1. Docker Swarm from docker
2. Kubernetes from Google
3. MESOS from Apache

###### Kubernetes 
In docker you might run one instance of container like this:
`docker run my-web-server`

but in kubernets you run multiple instance of them like this:
`kubectl run --replicas=1000 my-web-server`

you can scale them if 1000 arent enough
`kubectl scale --replicas=2000 my-web-server`

you can even roll update into them 
`kubectl rolling-update my-web-server --image-web-server:2`

or rollback updates if it made them unstable
`kubectl rolling-update my-web-server --rollback`

**1. Node** - Is a machine physical or virtual on which kubernetes and its set of tools installed.
	In nodes there are worker machines where containers will be launched by kubernetes.
**2. Cluster** - Is a set of nodes grouped together so even if one node fails your application will be still accessible by other active nodes.
**3. Master** - Is a node where kubernetes control plane component installed. Master manages the nodes and it responsible for actual orchestration of containers on the worker nodes. 

Some commands of kubernetes

Run a cluster
`kubectl run hello-mincube`

Get Cluster info
`kubectl cluster-info`

get nodes
`kubectl get nodes`

run 100 instances of *my-web-app* container images
`kubectl run my-web-app --image=my-web-app --replicas=100`

