# docker-compose: Node-RED

This Docker-Compose file contains a simple deployment of Node-RED. No credentials or similar are stored.

## Prerequisits
First a network must be created that can use Node-RED and services.
```bash
fheinke@github:~ # docker network create node-red-net
```

## Run Docker-Compose
The container can then be built via the docker-compose command.
```bash
fheinke@github:~ # cd PATH-TO-THE-docker-compose.yml
fheinke@github:~ # docker-compose up --build -d
```

For more information, the start logs can be viewed
```bash
fheinke@github:~ # docker-compose logs
```

If you want to start or stop the Container, you can simply do:
```bash
fheinke@github:~ # docker-compose stop
fheinke@github:~ # docker-compose start
```