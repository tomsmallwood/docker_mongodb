# Introduction
Docker image to install & manage a custom MongoDB cluster in Docker, with Kubernetes/ Docker Compose/ Swarm.

# Setup
## Docker Run
`docker login`
`docker build --tag=latest`
`docker run -p 4000:80 tsmallwood/mongodb_cluster:latest`

## View app
`localhost:4000`

## Docker Swarm
`docker swarm init` - Initialise swarm for docker
`docker stack deploy -c docker-compose.yml getstartedlab` - Start load-balanced swarm app
`docker service ps getstartedlab_web` - Get state of process "getstartedlab_web"
`docker service ls` - List docker swarm processes
`docker stack rm getstartedlab` - Take down getstartedlab app from docker swarm stack
`docker swarm leave --force` - Take down swarm. Now run `docker swarm init`

## Commands
`docker container ls` | List all running containers
`docker image ls -a`| List all images on this machine

# Description
## app.py
Temporary python app that we will replace with a MongoDB node.

## requirements.txt
Requirements for app.py (e.g. Regis, Flask)


# References
https://docs.docker.com/get-started/
https://docs.docker.com/docker-for-mac/
https://www.dlighthouse.co/2018/03/creating-mongo-replicaset-using-docker.html
https://www.youtube.com/watch?v=mlw7vWISaF4
