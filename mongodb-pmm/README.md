# MongoDB PMM Docker Setup
This project aims to start a mongodb replica set, where each mongo process has a connected pmm-client app, and also sets up a 

## Start Containers
Start docker containers using docker-compose. From inside this directory, run:
```
docker-compose up -d
```

## Update Containers
Stop the container required
```
docker stop <Container ID>
```
Recreate docker containers 
```
docker-compose up -d 
```


## Clean Up Project from Docker
```
docker rm $(docker ps -aq) -f
docker image rm perconalab/pmm-client mongo percona/pmm-server:1 --force
docker network rm docker_gwbridge mongodb-pmm_default
```