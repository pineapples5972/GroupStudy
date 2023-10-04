---
tags:
  - docker_notes
  - docker_commands
---
### Docker Dashboard
- [[Docker Resources]]
- [[DevOps Interview Questions]]
## Basics
#### What is container?
- its layers of images
- Linux based image
- and application image on top
#### Before Installing docker
##### steps to do on ubuntu
- [ ] check out distro release - `cat /etc/*release*`
- [ ] uninstall older versions - `sudo apt-get remove docker-engine docker.io containerd runc`
- [ ] uninstall older versions - `sudo apt-get remove docker-engine docker.io containerd runc`
- [ ] install docker through convenient script - `curl -fsSL https://get.docker.com -o get-docker.sh && sudo sh get-docker.sh`
- [ ] checking the installation - `sudo docker run docker/whalesay cowsay boo`
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

#### Creating your own custom image
#### Dockerfile
```
FROM Ubuntu
 1. OS - Ubuntu
RUN apt-get update
2. Update apt repo
RUN apt-get install python
RUN pip install flask
 3. Install dependencies using apt
RUN pip install flask-mysql
4. Install Python dependencies using pip
COPY . /opt/source-code
ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run
``` 

`docker build Dockerfile –t yourusername/my-custom-app`
`docker push yourusername/my-custom-app` 
_This will upload your dockerfile to docker registory online such as dockerhub.io_

#### Docker Registry

`docker run nginx`
_In this case nginx is repository name where it pull from the remote location such docker hub.
When user give image name to pull it actually expect account name and repo name like this:_
`docker run nginx/nginx`
_In this case first nginx is account name and second is repo name. When user type of only one name i.e. `docker run nginx` it assumes that both account name and repo name is that same name._
##### Public Registry
There are popular public docker repositories:
- gcr.io - google repository
- hub.docker.io - dockers official repository  
##### Private Registry
There are self hosted or private registries to chose from incase you dont want public exposure. Many cloud providers gives you private registries :
- AWS
- Azure
- JCP

To run a container from private registry you need to first login to with following command :
`docker login private-registry.io`

then you can run your private repository:
`docker run private-registry.io/apps/internal-app`

#### Deploy private registry
What if you running your application on premise and dont have private registry how do you deploy your own private registry within your organization:

The docker registry is itself is an application and it available inside docker image. The name of the image is registry. It exposes the api on port 5000.

`docker run -d -p 5000:5000 --name registry registry:2`
Now that you have your custom registry running on port 5000 on  how do you push your own image to it.

`docker image tag my-name localhost:5000/my-image`

`docker push localhost:5000/my-image`

Now you can pull this image anywhere within this network with server name i.e. localhost or ip address of the host.

`docker pull localhost:5000/my-image`

`docker pull 192.168.56.100:5000/my-image`

**Example:**
_To push some images into our locally hosted registry we can first pull then, tag them and then push them like this:_
Pulling:
`docker pull nginx:latest` 
then Tagging:
`docker image tag nginx:latest localhost:5000/nginx:latest` 
and finally push it: 
`docker push localhost:5000/nginx:latest`.  
To check the list of images pushed , use: `curl -X GET localhost:5000/v2/_catalog`

To remove all locally non running images
`docker image prune -a`

Now we can pull images from our `registry-server` as well. Use `docker pull [server-addr/image-name]` to pull the images that we pushed earlier.

In our case we can use: `docker pull localhost:5000/nginx`

