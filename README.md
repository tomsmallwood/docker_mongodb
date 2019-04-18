# Introduction
Docker image to install & manage a custom MongoDB cluster in Docker, with Kubernetes/ Docker Compose/ Swarm.

# Setup
## Docker Run
`docker login`
`docker run -p 4000:80 tsmallwood/mongodb_cluster:latest`

## View app
`localhost:4000`

# Description
## app.py
Temporary python app that we will replace with a MongoDB node.

## requirements.txt
Requirements for app.py (e.g. Regis, Flask)


# References
https://docs.docker.com/get-started/
https://docs.docker.com/docker-for-mac/
https://www.dlighthouse.co/2018/03/creating-mongo-replicaset-using-docker.html
