# MongoDB PMM Docker Setup
This project aims to start a mongodb replica set, where each mongo process has a connected pmm-client app, and also sets up a 

## Start Containers
Start docker containers using docker-compose.
```
docker-compose up -d
```

## Clean Up Project from Docker
```
docker stop $(docker ps -a -q) -t 0
docker rm $(docker ps -aq)
docker image rm perconalab/pmm-client mongo percona/pmm-server:1 --force
docker network rm docker_gwbridge mongodb-pmm_default
```