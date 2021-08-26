## show docker version information
docker version

## display system-wide information
docker info

## show docker disk usage
docker system df

## list running containers
docker ps

## list all containers
docker ps -a

## search image
docker search alpine
docker search --filter is-official=true mysql

## pull image
docker pull alpine

## list images (hide intermediate images)
docker images

## save image content
docker save <image name> > backup.tar

## list all images
docker images

## run new container (with interactive mode) (name: test)
docker run -it --name test alpine sh

## remove <test> container
docker rm test

## run new container (with interactive mode) and delete after when it exits
docker run -it --name test --rm alpine sh

## create a new image from a container's changes
docker commit f0eaa1bbd054 alpine/sh

## remove one or more images
``docker rmi <name> or <id>

## run new container from own image
docker run -it --rm --name test alpine/sh sh

## connect to interactive container
docker attach test  
(default detach keys: CTRL-p CTRL-q)

## connect to interactive container and set unique detach keys
docker attach --detach-keys="ctrl-a,ctrl-d" test

## stop one or more running containers
docker stop <name> or <id>

## start one or more stopped containers
docker start <name> or <id>

## change restart-policy
docker update <name> --restart always  
docker update nextc --restart always

## container logs/output
docker logs <name> or <id>

## show container port mapping
docker port <name> or <id>

## display the running processes of a container
docker top <name> or <id>

## display a live stream of container(s) resource usage statistics
docker stats <name> or <id>

## custom image from Dockerfile

contents of websrv directory
####
Dockerfile:
####

FROM alpine  
MAINTAINER Test1 Test2 <test1@test2.com>  
RUN /sbin/apk update  
RUN /sbin/apk upgrade  
RUN /sbin/apk add apache2  
RUN /bin/rm /var/www/localhost/htdocs/index.html  
COPY index.html /var/www/localhost/htdocs/index.html  
RUN /usr/sbin/httpd -k start  

####
start.sh:
####

#!/bin/sh  
/usr/sbin/httpd -k start  
/bin/sh

####
index.html:
####
Success

## build an image from a Dockerfile (-t <tag>) (websrv/ directory, where Dockerfile and others stored)
docker build -t alpine/websrv websrv/ 

## run container from custom image
docker run --name test2 -d -t alpine/websrv /start.sh

## port mapping (host 80, container 80)
docker run --name test2 -d -t -p 80:80 alpine/websrv /start.sh  
docker run --name test2 -i -t -p 80:80 alpine/websrv /start.sh

## run a command in a running container
docker exec test2 tail -f /var/log/apache2/access.log

## container low-level informations
docker inspect <name> or <id>

## container ip address
docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <id>

## limit memory 
docker update <name> --memory=200m --memory-swap=200m
  
  
/etc/subuid  
/etc/subgid
