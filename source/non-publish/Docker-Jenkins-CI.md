title: 'Docker & Jenkins: CI'
date: 2016-01-15 16:08:03
tags: Docker, Jenkins, CI
toc: true
---


Docker is great at building and sharing disk images with the others through the docker index. It can be taken into consideration as a `Manager for infrastructure`. Docker has a great image distribution model fro server templates build with configuration manageer like `chef`, `puppet`, `SaltStack`, etc. The great thing from Docker is that it has a central repository of disk images, which are built from million developers all over the world.

{% img [docker] http://blog.docker.com/wp-content/uploads/2013/08/KuDr42X_ITXghJhSInDZekNEF0jLt3NeVxtRye3tqco.png %}



## Docker CLI & Docker Image
The common used commands of docker CLI are: `daemon`, `run`, `stop`, `build`, `exec`, `logs`:

### common-used
```bash
docker ps # show only active containers
docker ps -aq # show only active container IDs
docker images # show only active images
docker images -a # show ALL images
docker images -aq # show ALL images IDs

docker rm {containerId} # delete container by ID
docker rmi {imageId} # delete image by ID
docker rm -f $(docker ps -aq) # delete all containers
docker rmi -f $(docker images -aq) # delete all images 
```

### daemon
the docker daemon is the persistent process that manages containers.
```bash
docker daemon -D
```

### run
```bash
docker run -p 172.28.128.10:8080:80 ubuntu bash
```
this binds port `8080` of machine with IP `172.28.128.10` to the port of `ubuntu contianer`. If you have web application installed in ubuntu container, which is running on port 80. From your host machine, i.e. from my macbook, I can access the web app in `ubuntu container` via http://172.28.128.10:8080


### stop

### build

```bash
docker run --help # get help info
docker --entrypoint=
```


### exec

### logs




## Docker Hub
foo

## Docker composer
foo

## CI Plattform with Jenkins
foo

## Boilerplate for Mac User
foo