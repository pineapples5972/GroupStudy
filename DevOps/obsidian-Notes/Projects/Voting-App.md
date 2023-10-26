---
tags:
  - docker
  - docker_notes
  - project
---

**Project Source Code:** https://github.com/dockersamples/example-voting-app.git

#### Tasks:
- [x] Git Clone the Repository
- [x] build docker image using repo
- [x] run the container
- [x] access the webapp through browser
- [ ] resolve the issues (if found)
- [ ] run with redis attached
- [ ] run with worker app and postgres
- [ ] run with result app
- [ ] customize the app
- [ ] rerun the app

#### All Commands As per [Video 1](https://kodekloud.com/topic/demo-example-voting-application-with-docker-compose/): 
- Git Clone: `git clone https://github.com/dockersamples/example-voting-app.git`  
- docker build: `docker build . -t voting-app`
###### Running Only voting-app:
- docker run: `docker run -p 5000:80 voting-app`
- https://localhost:5000 
###### Running with Redis attached:
- `docker pull docker.io/redis`
- `docker run -d --name=redis redis`
- `docker run -p 5000:80 --link redis:redis voting-app`
###### Running with postgres and worker-app
- `docker run -d --name=db postgres:9.4`
- navigate to worker directory `cd ../worker `
- build the worker: `docker build . -t worker-app`
- run worker container: `docker run --link redis:redis --link db:db worker-app`
###### Running with result-app finally
- navigate to result directory: `cd ../result`
- build image: `docker build . -t result-app`
- run the result container: `docker run -p 5001:80 --link db:db result-app`
- see result page at: https://localhost:5001


