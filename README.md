# Wildfly - Mysql Stack in Cloud

This instructions allow the creation of the the environment for Qualibrate Foundation Platforn in a cloud environment.

## Setup
1. Install docker-machine
2. Install VirtualBox
3. Get AWS Credentials

```sh
[default]
aws_access_key_id=YOUR_KEY
aws_secret_access_key=YOUR_PASS
```

## Build up first machine

```docker
# Launching first machine
REGION: eu-central-1
ZONE: b
SUBNET:
docker-machine create --driver amazonec2 --amazonec2-region aws01
```


## Installing services
Inside your machine manager and nodes, you can create the swarm with
```sh

docker swarm init --advertise-addr 192.168.99.100


    docker swarm join \
        --token SWMTKN-1-5nyfjhvfrnlf4ky53muwq9jvtk1jdctec2rpmoxv51xgmiyi06-ecf2g4czxpoptcf6xcemxkqb7 \
        192.168.99.100:2377


docker service create --network selnet --name hub -p 4444:4444 selenium/hub:latest
docker service create --network selnet --name node-chrome --mount type=bind,source=/dev/shm,target=/dev/shm -e HUB_PORT_4444_TCP_ADDR=hub -e HUB_PORT_4444_TCP_PORT=4444 -e NODE_MAX_INSTANCES=1 -e NODE_MAX_SESSION=1 --replicas 1 selenium/node-chrome:latest bash -c 'SE_OPTS="-host $HOSTNAME" /opt/bin/entry_point.sh'

```
