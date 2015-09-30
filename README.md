# acivemq-websocket-example
`sh activemq-websocket.sh`

This is a simple script that run for you 1 docker images:
- Jbosse fuse (**you need to build this image yourself**): https://github.com/paoloantinori/dockerfiles/tree/master/centos/fuse

After that it creates a fabric, a child container, an mq-websocket profile and finally apply the profile to the child container in order to have an activeMQ broker container configured with stomp and wensocket connectors both with and without SSL.

## Interacting with the Fuse container
When the script finish you should be able to check fuse container's local ports with:
```
$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                                                                                                                                                  																	NAMES
2d28823d8415        fuse6.2:latest      "/bin/sh -c 'service   10 minutes ago      Up 10 minutes       0.0.0.0:49200->22/tcp, 0.0.0.0:49201->44444/tcp, 0.0.0.0:49202->8181/tcp, 0.0.0.0:49203->1099/tcp, 0.0.0.0:49204->8101/tcp, 0.0.0.0:61614->61614/tcp, 0.0.0.0:61615->61615/tcp, 0.0.0.0:61616->61616/tcp, 0.0.0.0:61617->61617/tcp, 0.0.0.0:61618->61618/tcp, 0.0.0.0:61619->61619/tcp   root
```
in this example the hawtio console would be at `http://localhost:49202`, karaf console at `localhost:49204` and ssh into the container at `localhost:49200`.

### List of activeMQ connectors
Referring to the previous example you'd have the following activeMQ connectors accessible:
- openwire connector at `localhost:61616`
- secure openwire connector at `localhost:61617`
- stomp connector at `localhost:61618`
- secure stomp connector at `localhost:61619`
- websocket connector at `localhost:61614`
- secure websocket connector at `localhost:61615`

## NOTE Before launching the script:
Before launching the script you need to build fuse6.2 image yourself by download JBoss Fuse distribution from

http://www.jboss.org/products/fuse

The build process will extract in the Docker image all the zip files it will find in your working folder. If it finds more than a file it will put all of them inside the  Docker it's going to be created. Most of the time you will want to have just a single zip file.

## To build your Fuse image:
    # download docker file
	wget https://raw.github.com/paoloantinori/dockerfiles/master/centos/fuse/fuse/Dockerfile

    # check if base image has been updated
	docker pull pantinor/fuse

    # build your docker fuse image. you are expected to have either a copy of jboss-fuse-full-6.2.0.redhat-133.zip or a link to that file in the current folder.
    docker build --rm -t fuse6.2 .
