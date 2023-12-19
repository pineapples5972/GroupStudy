original project link - https://git.sr.ht/~heckyel/yt-local

What is Yt-local?
Its an bloatless frontend for youtube self hostable written in pyhon and flash framework uses very little footprint and allows tor routing for annonymous surfing of youtube.

==Original yoututbe platform is bloated and very slow to use on low end machines thats why this project came into existence==

Can be deployed using this docker image: 
https://hub.docker.com/r/rusian/yt-local

deployed on port 8080 by default

build image:
`docker build -t yt-local .`

Run with:
`docker run -d -p 8080:8080 yt-local`

access on:
http://localhost:8080

Here is the dockerfile extracted from docker hub
Dockerfile of rusian/yt-local image
``` .Dockerfile
FROM python:3.10-alpine AS builder

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

# Create a virtual environment
RUN python3 -m venv venv

# Install dependencies into the virtual environment
RUN . venv/bin/activate && pip install -r requirements.txt

# Switch to the virtual environment for the rest of the build
WORKDIR venv/bin

# Expose the application port
EXPOSE 8080

# Copy the server file
COPY server.py .

# Run the application
CMD ["./", "python3", "server.py"]
```
