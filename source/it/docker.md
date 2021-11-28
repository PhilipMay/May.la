# Docker
- build: `docker build . -t <tag_name>`
- connect to a container: `docker exec -it <container_name> bash`
- delete all: `docker stop $(docker ps -aq) && docker rm $(docker ps -aq) && docker rmi $(docker images -q)`

## Dockerfile
- set variables: `ARG variable=value`
- use variables example: `WORKDIR /home/$variable`
