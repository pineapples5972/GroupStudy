Reference: https://www.youtube.com/watch?v=JKxlsvZXG7c

Task is simple - Just host the simple index.html page with nginx server.

- require nginx package: `docker pull nginx`
- run it on port 5003: `docker run -d -p 5000:80 nginx`

Todo:
- [ ] Run Nginx container
- [ ] Successfully navigate to http://localhost:5003
- [ ] configure nginx serving location to /app/www/
- [ ] successfully run our page on browser


