# Docker

## Overview

### Docker Concepts

### Docker file

### Docker Files, Images and Containers

### Docker Volumes
## Commands

#### Images
| Command      | Description |
| :---        |    ---:   |
|```docker build  -t <tag> <path-dockerfile>```| Build a docker image (Using a Docker File) |
|```docker images```| Check all Docker Images |
|```docker pull <img-name>```| Pull a docker image from docker registry |

#### Containers
| Command      | Description |
| :---        |    ---:   |
|```docker rmi <img-id>```| Delete an image |
|```docker run <img-name>```| Run a docker container (Ship a container from a docker image) |
|```docker run -it <img-name>```| Run interactively |
|```docker run -d  <img-name>```| Run on the background |
|```docker run --gpus all  <img-name>```| Run with gpus |
|```docker run -d  <img-name>```| Run with devices etc |
|```docker exec -it <container-id> bash```| Enter running container console |
|```docker -ps```| Check running containers |
|```docker ps -a```| Check all containers running or stopped |
|```docker logs --tail N <container-id>```| Check container logs |
|```docker stop <container-id/container-name>```| Stop a Docker Container |
|```docker rm <container-id/container-name>```| Destroy a container |
|```docker rm -f $(docker ps -a -q)```| Destroy all containers |
|```docker rm $(docker container ls --filter "status=exited" -q)```| Destroy all stopped containers |

#### Volumes
| Command      | Description |
| :---        |    ---:   |
|```docker volume ls```| list all volumes |
|```docker volume rm <volume>```| Destroy a volume |
|```docker volume rm $(docker volume ls -q)```| Delete all volumes |

#### System

| Command      | Description |
| :---        |    ---:   |
|```docker --version```| Check Docker Version |
|```docker container prune -f```| Frees unused containers |
|```docker image prune -f```| Frees unused cached images |
|```docker volume prune -f```| Frees unused volume space |
|```docker system prune```| Frees unused cached space |

### Docker Compose
We construct a docker compose using the yml syntax. the first label should be the version of our code (it's optional). then we can define the services. each service its an application, like a docker container. for example its common to have 2 services, one our server app and the other one the container of the DB. <br>
<br>
another label could be the definition of volumes. a common practice its to have a volume for our database. since our database its a container, once its stopped the data will be lost and would defeat the purpose of a database, so we have to persist it and we do it using volumes. we map a place on the host machine to the db container and it will store the data of our db.<br>

#### Docker compose commands
| Command      | Description |
| :---        |    ---:   |
|```docker compose up```| Runs containers as described on the docker-compose file |
|```docker compose up -d```| Runs containers as described on the docker-compose file (Detached) |
|```docker compose down```| Stops, and removes containers as described on the docker-compose file |

### Docker Networking
A first example of networking would be to connect to a database running on the host machine. As docker is kind of like a VM, it can't directly access the localhost (the host machine), for that we can use a special variable that docker uses, and instead of using localhost we would use this variable:

docker run --add-host host.docker.internal:host-gateway  -e DATABASE_URL=<dburl> <img_name>

however this sometimes doesn't work. another option is to set our docker as network mode host and this way we can use localhost to access the host's localhost ip.

docker run --network=host -e DATABASE_URL=<dburl> <img_name>

For docker compose
network_mode: host