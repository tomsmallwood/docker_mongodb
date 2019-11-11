# MongoDB PMM Docker Setup
This project aims to start a mongodb replica set, where each mongo process has a connected pmm-client app, and also sets up the pmm-server application to monitor MongoDB Cluster metrics. 


## Docker-Compose
I have found that starting a mongodb cluster using docker-compose poses a few problems, such as: inability to use `docker-compose scale` when connecting the pmm-client to a mongod with different port numbers depending on the port number generated at `run-time` by `docker-compose scale`.
Solution for this would be to create my own dockerfile, with pmm-client and mongodb in one image, so that port numbers can be defined by docker before run-time, and used between two seperate apps in the same docker image, however, this architecture goes against Docker's inherent microservices desgin.
Another solution is to use Ansible to generate individual docker containers using Ansible 'list' or 'object' logic. We can repeatably create docker containers based on a list of ports (for example), using the Ansible module `with_items`. This will pass `item.port` into pmm-client container's 'command' option, allowing it to create multiple pmm-client containers which will connect to the specific port respecitvely, as provided by that list. This also gets around having to write/maintain my own Dockerfile.

### Start Containers
Start docker containers using docker-compose. From inside this directory, run:
```
docker-compose up -d
```

### Re-create Container
Required after modifying the docker-compose file.
This may require deleting the old ./data/ directories to fix mongodb lock. 
Recreate docker containers: 
```
docker-compose up -d 
```

### Stop Container
Stop the container:
```
docker stop <Container ID>
```
Stop all containers:
```
docker stop $(docker ps -aq)
```

### Clean Up Project from Docker
```
docker rm $(docker ps -aq) -f
rm -rf ./data/configsvr/*
rm -rf ./data/mongod/*
rm -rf ./data/mongos/*
docker image rm perconalab/pmm-client mongo percona/pmm-server:1 --force
docker network rm docker_gwbridge mongodb-pmm_default
```

### Setup CLuster
REFERENCE: https://dzone.com/articles/composing-a-sharded-mongodb-on-docker

Initiate Configsvr Replica Set:
```
docker exec -it mongodb-pmm_config-1_1 bash -c "echo 'rs.initiate({_id: \"configReplSet\",configsvr: true, members: [{ _id : 0, host : \"config-1:27029\" },{ _id : 1, host : \"config-2:27129\" }, { _id : 2, host : \"config-3:27229\" }]})' | mongo --port 27029 -u mongo -p secret --authenticationDatabase admin"
```
Or via the container:
```
rs.initiate({_id: "configReplSet", configsvr: true, members: [{ _id : 0, host : "config-1:27029" },{ _id : 1, host : "config-2:27129" }, { _id : 2, host : "config-3:27229" }]})
```

## Ansible Compose
After seeing issues with creating docker containers in docker-compose. I chose to test out using Ansible to do the same thing. I have previous experience using Ansible to build a MongoDB cluster in Linux, however, this should remove a lot of complexity in configuring a VM for this specific use.

### Installing docker module for Ansible
`docker` module is deprecated, instead we use dedicated: `docker_container` or `docker_image`.

However, I currently have issues with pip, where 'module' object is not callable, so I wil have to check what this issue is before I can continue using Ansible.