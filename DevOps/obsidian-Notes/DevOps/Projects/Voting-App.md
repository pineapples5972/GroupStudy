---
tags:
  - docker
  - docker_notes
  - project
---
![[voting-app.excalidraw]]
**Project Source Code:** https://github.com/dockersamples/example-voting-app.git

#### Tasks:
- [x] Git Clone the Repository
- [x] build docker image using repo
- [x] run the container
- [x] access the webapp through browser
- [x] resolve the issues (if found)
- [x] run with redis attached
- [x] run with worker app and postgres
- [ ] run with result app
- [ ] customize the app
- [ ] rerun the app

#### All Commands As per [Video 1](https://kodekloud.com/topic/demo-example-voting-application-4/): 
- Git Clone: `git clone https://github.com/dockersamples/example-voting-app.git`  
- navigate to voting-app directory: `cd vote`
- docker build: `docker build . -t voting-app`
###### Running Only voting-app:
- docker run: `docker run -d -p 5000:80 voting-app`
- https://localhost:5000 
###### Running with Redis attached:
- Pull Redis image: `docker pull docker.io/redis`
- Run Redis Container in bg: `docker run -d --name=redis redis`
- Link redis container with voting app network: `docker run -p 5000:80 --link redis:redis voting-app`
###### Running with postgres and worker-app
- Pull Postgres image: `docker run -d --name=db -e POSTGRESS_PASSWORD=password postgres:9.4`
- navigate to worker directory `cd ../worker `
- build the worker: `docker build . -t worker-app`
- run and link db to redis and worker: `docker run --link redis:redis --link db:db worker-app`
###### Running with result-app finally
- navigate to result directory: `cd ../result`
- build image: `docker build . -t result-app`
- run the result container: `docker run -p 5001:80 --link db:db result-app`
- see result page at: https://localhost:5001

![[Pasted image 20231103131614.png]]
![[Pasted image 20231103131644.png]]

### Running with docker-compose

docker-compose.yml file
```
# version is now using "compose spec"
# v2 and v3 are now combined!
# docker-compose v1.27+ required

services:
  vote:
    build: 
      context: ./vote
      target: dev
    depends_on:
      redis:
        condition: service_healthy
    healthcheck: 
      test: ["CMD", "curl", "-f", "http://localhost"]
      interval: 15s
      timeout: 5s
      retries: 3
      start_period: 10s
    volumes:
     - ./vote:/usr/local/app
    ports:
      - "5000:80"
    networks:
      - front-tier
      - back-tier

  result:
    build: ./result
    # use nodemon rather than node for local dev
    entrypoint: nodemon --inspect=0.0.0.0 server.js
    depends_on:
      db:
        condition: service_healthy 
    volumes:
      - ./result:/usr/local/app
    ports:
      - "5001:80"
      - "127.0.0.1:9229:9229"
    networks:
      - front-tier
      - back-tier

  worker:
    build:
      context: ./worker
    depends_on:
      redis:
        condition: service_healthy 
      db:
        condition: service_healthy 
    networks:
      - back-tier

  redis:
    image: redis:alpine
    volumes:
      - "./healthchecks:/healthchecks"
    healthcheck:
      test: /healthchecks/redis.sh
      interval: "5s"
    networks:
      - back-tier

  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_USER: "postgres"
      POSTGRES_PASSWORD: "postgres"
    volumes:
      - "db-data:/var/lib/postgresql/data"
      - "./healthchecks:/healthchecks"
    healthcheck:
      test: /healthchecks/postgres.sh
      interval: "5s"
    networks:
      - back-tier

  # this service runs once to seed the database with votes
  # it won't run unless you specify the "seed" profile
  # docker compose --profile seed up -d
  seed:
    build: ./seed-data
    profiles: ["seed"]
    depends_on:
      vote:
        condition: service_healthy 
    networks:
      - front-tier
    restart: "no"

volumes:
  db-data:

networks:
  front-tier:
  back-tier:
```

run `docker-compose up`
check out : http://localhost:5000 
and http://localhost:5001

And this project is done.

---

![[Pasted image 20231028161523.png]]
#### Setup for Podman:
 %%In Podman, the ==`--link` flag is not available==. Podman provides a different approach for container networking. To replicate your project setup without the `--link` flag, you can use Podman pods and user-defined networks. Also Since we need two pods to communicate with each other we should create common network %%
 
![[Two-pods-communicating.excalidraw]] 
 
 0. **Create a common network for both our pods:**
   `podman network create my-network` 

1. **Create a Pod and Specify Port Mapping:**
   `podman pod create --name my-pod -p 5000:80 --network my-network`

2. **Run Containers Inside the pod:**
```
podman run -d --name redis --pod my-pod redis
podman run -d --name voting-app --pod my-pod voting-app
podman run -d --name db -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres --pod my-pod -v /data:/var/lib/postgresql/data postgres
podman run -d --name worker-app --pod my-pod worker-app 
```

%% In Podman, you cannot specify port mappings for individual containers within a pod. Instead, you need to define port mappings for the entire pod when you create it. To work around this limitation, you can create a new pod with the desired port mapping and then move your existing containers into the new pod. %%

3. **Create a new pod with the desired port mapping:**
  `podman pod create --name my-new-pod -p 5001:80 --network my-network`
  
1. **Start the "result-app" container inside the new pod:**
  `podman run --name result-app --pod my-new-pod result-app`
  or try this:
  `podman run -p 5001:80 --name result-app --pod my-new-pod --network my-pod result-app`

%% The above commands may launch result app but it cant count votes. The problem with podman is that it doesnt allow communicating of two pods so it is neccessary to use kubernetes in this case %%

The Issue is Postgres is failing, because its database is not initiated.
Its should be initialized with these commands:

```
CREATE DATABASE mydatabase; 
CREATE USER myuser WITH PASSWORD 'mypassword'; 
GRANT ALL PRIVILEGES ON DATABASE mydatabase TO myuser;
```

Update: Even tho after successfully solving postgress error it still not seems to communicate with pod 1.