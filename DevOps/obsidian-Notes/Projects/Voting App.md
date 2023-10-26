---
tags:
  - docker
  - docker_notes
  - project
---
Source Code Repo: https://github.com/dockersamples/example-voting-app.git

Tasks:
- [ ] Git Clone the Repository
- [ ] build docker image using repo
- [ ] run the container
- [ ] access the webapp through browser
- [ ] resolve the issues (if found)
- [ ] customize the app
- [ ] rerun the app

- Git Clone: `git clone https://github.com/dockersamples/example-voting-app.git`  
- docker build: `docker build . -t voting-app`
- docker run: `docker run -p 5000:80 voting-app`
- https://localhost:5000 
- docker pull docker.io/redis
- docker run -d --name=redis redis
- docker run -p 5000:80 --link redis:redis voting-app
- 