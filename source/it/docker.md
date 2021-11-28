# Docker
- build: `docker build . -t <tag_name>`
- connect to a container: `docker exec -it <container_name> bash`
- delete all: `docker stop $(docker ps -aq) && docker rm $(docker ps -aq) && docker rmi $(docker images -q)`
- list containers - [docker ps](https://docs.docker.com/engine/reference/commandline/ps/)
  - list running containers: `docker ps`
  - list all containers: `docker ps -a`

## Dockerfile
- set variables: `ARG variable=value`
- use variables example: `WORKDIR /home/$variable`

## docker-compose
- store config in `docker-compose.yml` by defaut
- start (build images before starting containers): `docker-compose up --build` - [docker-compose up](https://docs.docker.com/compose/reference/up/)
  - add `-d` for detached mode
- stop and remove containers: `docker-compose down` - [docker-compose down](https://docs.docker.com/compose/reference/down/)
