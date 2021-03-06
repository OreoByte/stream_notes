# How to convert a active docker container into a static docker image #

# creating the joke alias of doker for docker for the current user

```bash
cp ~/.bashrc ~/.bashrc.bak
# nano or vim ~/.bashrc
# add this line
# alias <alias-name>="<command-sting-you-want-to-the-alias-to-use>"
alias doker="docker"

# update the alias
source ~/.bashrc

# test

doker images
```

---

# create new docker containers

## with docker on liners line commands

## with docker files

* docker file that will specify docker image and commands run on the container

### quick alpine example

```bash
FROM alpine:latest
RUN apk update && apk add wget
RUN apk add tmux

RUN useradd -ms /bin/bash oreobyte
RUN echo -e "P@ssw0rd\P@ssw0rd" | (passwd --stdin oreobyte)
RUN usermod -aG sudo oreobyte
RUN passwd --expire oreobyte
```

### quick ubuntu example

```bash
FROM ubuntu:latest
RUN apt update
RUN apt install tmux

RUN useradd -ms /bin/bash oreobyte
RUN echo -e "P@ssw0rd\P@ssw0rd" | (passwd --stdin oreobyte)
RUN usermod -aG sudo oreobyte
RUN passwd --expire oreobyte
```

* docker build file

```bash
#!/bin/bash
#docker build -t <new-container-name> .
docker build -t tmux .

<<comment
NOTE: both (dockerfile and build file must be in the same directory)

chmod +x build.sh && ./build.sh
comment
```

## how to over come the `TimeZone` pause

### Solution for ubuntu machines. NOT sure if it will work with other distros

https://serverfault.com/questions/949991/how-to-install-tzdata-on-a-ubuntu-docker-image

* add this to the intended dockerfile

```bash
ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=<region/city>
RUN apt-get install -y tzdata
```

```bash
ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Europe/Moscow
RUN apt-get install -y tzdata
```

---

# nuke all docker containers
	`docker system prune -a`

	* cleans out all data from /usr/local/docker/data I forget the path

0r

	`docker rmi $(docker image -qa)`
	
# not nuke notes (for containers that have already been installed)

## Get all active docker image ID hashes
	docker image -qa

## return all docker image currently installed and setup, but not active
	`docker images | tail -n +2 | awk '{print $1}'`

## list all active docker conatiners
	`docker ps`

## enter docker container
	`docker run -it --rm c059bfaa849c /bin/sh`
	`docker run -it --rm ba6acccedd29 /bin/bash`

## background as a linux daemon
	`docker run -d <image-name>`
	`docker run -d d0e80c6435b5`

	* NOTE: didn't really work during stream. Must be researched again.

## docker stop container id
	`docker stop d0e80c6435b5`

--------------------------------
# backup and restore active docker containers within ps (process list)

1. convert active container into staticly listed docker image
	`docker commit <container-id-hash> <repo-name>:<tag-name>`

	`doker commit 8d0849d97765 alpine/oreo:xayah-ascii`

2. double check (that image has been made)
	`docker images`

```bash
??????$ docker images
REPOSITORY    TAG           IMAGE ID       CREATED             SIZE
alpine/oreo   xayah-ascii   1c9ed0284f18   2 minutes ago       54MB
<none>        <none>        ab90b1a2684b   58 minutes ago      12.2MB
<none>        <none>        969d3883371d   About an hour ago   142MB
<none>        <none>        0d85266c322c   About an hour ago   105MB
debian        latest        05d2318291e3   13 days ago         124MB
alpine        latest        c059bfaa849c   2 weeks ago         5.59MB
ubuntu        latest        ba6acccedd29   2 months ago        72.8MB
```

3. save the image from <docker images> into a backup.tar file
	`doker save alpine/oreo > alpine_oreo.tar`
0r
	`doker save alpine/oreo --output alpine_oreo.tar`


4. delete image or place on a new machine that doesn't have the docker image
	* not resquired but not sure if the new image name will break anything delete stuff at your own risk

	`docker rmi 1c9ed0284f18`

	`docker rmi --force <image_id>`
	`docker rmi --force 1c9ed0284f18`

	* NOTE; (only need force if left over docker layers are not removed)

* check for extra layers
	`docker images`
	`docker inspect <image_id_hash>`
	
	* each layer will have it's own sha256 hash 
	
* tool to fix layers for security and speed
	https://github.com/wagoodman/dive

	* NOTE; "WorkDir" is where it is hosted on disk

5. restore image from tar backup
	`docker load -i ./alpine_oreo.tar`

6. run again to check to make sure nothing is broken
	`docker run -it 1c9ed0284f18 /bin/sh`
	`docker run -it --rm 1c9ed0284f18 /bin/sh`

---

# have a docker container stay up/active. Even after a system reboot

* by modifying the `restart policy`

https://docs.docker.com/config/containers/start-containers-automatically/

## when the docker image in inactive

```
docker run -d --restart <policy-type> <REPOSITORY/container-name>
docker run -d --restart unless-stopped tester
```

## while the docker container is currently running/active

```
docker run update --restart <policy-type> <REPOSITORY/container-name>
docker run update --restart unless-stopped tester
```

## double check that it worked

1. restart machine
2. open terminal
	`docker ps`
		* if blank or asssigned container not found active = problem

---

# expose docker ports to the host

## expose the ports when starting docker containers

```
docker run -p <host-port-number>:<docker-container-port-number> -it -rm ubuntu:latest /bin/bash

docker run -p 80:80 -it --rm ubuntu:latest /bin/bash
docker run -p 80:8080 -it --rm ubuntu:latest /bin/bash
```

---

# useful docker commands that are not apart of a main command change but are useful to know

1. print the env of the docker container you are in
	`printenv`
