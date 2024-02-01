---
tags:
  - "#DevOps"
  - "#Docker"
  - "#Installations"
---

#### Before Installing docker
##### steps to do on ubuntu
- [ ] check out distro release - `cat /etc/*release*`
- [ ] uninstall older versions - `sudo apt-get remove docker-engine docker.io containerd runc`
- [ ] uninstall older versions - `sudo apt-get remove docker-engine docker.io containerd runc`
- [ ] install docker through convenient script - `curl -fsSL https://get.docker.com -o get-docker.sh && sudo sh get-docker.sh`
- [ ] checking the installation - `sudo docker run docker/whalesay cowsay boo`

#### Docker on windows

##### 1. Docker Toolbox
1. Oracle Virtualbox
2. Docker Engine
3. Docker machine
4. Docker compose 
5. Kitematic GUI

##### 2. Docker Desktop for windows
1. HyperV
2. Linux Distro
3. Docker engine

##### 3. Docker on Mac
1. HyperKit
2. Linux Distro
3. Docker Engine
