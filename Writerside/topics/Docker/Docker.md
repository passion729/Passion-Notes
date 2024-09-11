# Docker

## **Container and Image**

Image：Single file with all the devs and config required to run a program

Container：Instance of an image. Runs a program.

## Command

`docker run <image name> <initial command>` // Creating and running a container from an image

`docker run <image id> <initial command>`

`docker ps` // List all running containers

`docker ps --all` // List all containers' history

`docker = docker create + docker start`

`docker create <image name> <initial command>`

`docker start <container id>`

`docker start -a <container id>` // `-a`flag is used to print container's log to terminal

`docker system prune` // Clean up docker

`docker logs <container id>` // Get logs from a container, if you forgot add the`-a`flag

`docker stop <container id>` // Stop a container

`docker kill <container id>` // Kill a container

`docker exec -it <container id> <command>` // Excute an additional command in a container, `-it`flag allow us to provide input to the container

`docker exec -i <container id> <command>` // Standard input`docker exec -t <container id> <command>` // Standard output

`docker exec -it <container id> sh` // Go into terminal

`docker run -it <image name> <initial command>`

## **Docker file**

```other
# Use an existing docker image as a base
FROM alpine

# Set the work directory
WORKDIR /usr/app

# Download and install a dependency
COPY ./ ./
RUN apk add --update redis
RUN apk add --update gcc
RUN npm install

# Tell the image what to do when it starts as a container
CMD [ "redis-server" ]
CMD [ "npm", "start" ]
```

`docker build <directory>`

`docker build -t passion/redis:latest <directory>` // Docker ID/Project name:version`docker build -f <Dockerfile.dev> <directory>` // Specify the dockerfile

## **Docker Image**

`docker image ls` // List docker images

`apk add --update redis` // Add package in terminal environment

`docker commit -c 'CMD ["redis-server"]' <container id>` // Add a dependency to an image

What dose`alpine`mean?

`dockwer run -p <port on local>:<port inside container> <image name/id>` // Run with port mapping

## **Docker Compose**

Separate CLI that gets installed along with docker

Used to start up multiple docker containers at the same time

Automates some of the long-winded arguments we are passing to 'docker run

```yaml
version: '3'
services:
	redis-server:
		image: 'redis'
	node-app:
		build: .
	ports:
		- "8081:8081"
```

`docker-compose up` // docker run image

`docker-compose up --build` // docker build . & docker run image

`docker-compose up -d` // Launch in background

`docker-compose down` // Stop containers

`docker-compose ps`

[[Docker/Ubuntu22.04 on Docker]]

[MySql](craftdocs://open?blockId=E47DA563-A457-4EBB-ABC1-2D9ABE7C94DF&spaceId=541b7c88-d193-df3a-a475-0ade82a7d9d3)

[[Docker/RabbitMQ on Docker]]

[[Docker/Redis on Docker]]

