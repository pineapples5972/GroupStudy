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
 

 
   



