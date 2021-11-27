# Docker
- delete all: `docker stop $(docker ps -aq) && docker rm $(docker ps -aq) && docker rmi $(docker images -q)`
- connect to a container: `docker exec -it <container_name> bash`

## Dockerfile
- set variables: `ARG variable=value`
- use variables example: `WORKDIR /home/$variable`
