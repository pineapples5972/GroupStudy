![[nginx-webserver-docker.excalidraw]]

Task is simple - Just host the simple index.html page with nginx webserver, reconfigure it to reflect new changes successfully.

- require nginx package: `docker pull nginx`
- run it on port 8080: `docker run -d -p 8080:80 nginx`
- configure it and successfully deploy changes.

##### **Todo:**
- [x] Run Nginx container
- [x] Successfully navigate to http://localhost:5003
- [x] change configuration and reload the service
- [x] successfully run our page on browser

##### **Steps:**
##### 1.  Build an Image
Make a project Directory: 
`mkdir nginx-project `
add `Dockerfile` with following content in it:
`vim Dockerfile`
 ```
FROM docker.io/nginx:stable-alpine
RUN rm -rf /usr/share/nginx/html/*
COPY ./dist/app /usr/share/nginx/html
```

In the same directory make a directory tree `dist/app`
`mkdir -p dist/app`

and put `index.html` dummy file in it:
`vim dist/app/index.html`

put any html content on it like this:
```
<h1> Yoohoo, It working</h1>
<h2> Finally Sigh of relief </h2>
```

Finally Build image:
`docker build -t nginx:latest .`

run your docker image:
`docker run -d -p 8080:80 nginx`

##### 2. Entering into the container:
Now to edit things get into your container
`docker exec -it <id> /bin/sh`

make any changes in your `/etc/nginx/nginx.conf`

validate configuration:
`nginx -t`

If you seeing message like this without any error this means everything is okay.
```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

To apply configuration use:
`nginx -s reload`

If you see your changes took place and everything working well good job!

##### References: 
1. https://www.youtube.com/watch?v=JKxlsvZXG7c
2. https://dev.to/kutsyk/manage-nginx-configurations-inside-docker-container-9da