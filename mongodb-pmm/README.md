# MongoDB PMM Docker Setup
This project aims to start a mongodb replica set, where each mongo process has a connected pmm-client app, and also sets up a 

## Start Containers
Start docker containers using docker-compose. From inside this directory, run:
```
docker-compose up -d
```

## Re-create Container
Required after modifying the docker-compose file.
This may require deleting the old ./data/ directories to fix mongodb lock. 
Recreate docker containers: 
```
docker-compose up -d 
```

## Stop Container
Stop the container:
```
docker stop <Container ID>
```
Stop all containers:
```
docker stop $(docker ps -aq)
```

## Clean Up Project from Docker
```
docker rm $(docker ps -aq) -f
rm -rf ./data/configsvr/*
rm -rf ./data/mongod/*
rm -rf ./data/mongos/*
docker image rm perconalab/pmm-client mongo percona/pmm-server:1 --force
docker network rm docker_gwbridge mongodb-pmm_default
```

## Setup CLuster
REFERENCE: https://dzone.com/articles/composing-a-sharded-mongodb-on-docker

Initiate Configsvr Replica Set:
```
docker exec -it mongodb-pmm_config-1_1 bash -c "echo 'rs.initiate({_id: \"configReplSet\",configsvr: true, members: [{ _id : 0, host : \"config-1:27029\" },{ _id : 1, host : \"config-2:27129\" }, { _id : 2, host : \"config-3:27229\" }]})' | mongo --port 27029 -u mongo -p secret --authenticationDatabase admin"
```
Or via the container:
```
rs.initiate({_id: "configReplSet", configsvr: true, members: [{ _id : 0, host : "config-1:27029" },{ _id : 1, host : "config-2:27129" }, { _id : 2, host : "config-3:27229" }]})
```
