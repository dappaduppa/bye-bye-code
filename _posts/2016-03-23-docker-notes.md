---
layout: post
title:  "docker notes"
date:   2016-03-23 14:28:46 +0900
categories: go docker
---

https://github.com/docker/labs/tree/master/beginner

```sh
$ docker run hello-world


$ docker pull alpine


$ docker images
REPOSITORY              TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
alpine                 latest              c51f86c28340        4 weeks ago         1.109 MB
hello-world             latest              690ed74de00f        5 months ago        960 B

$ docker run alpine ls -l
total 48
drwxr-xr-x    2 root     root          4096 Mar  2 16:20 bin
drwxr-xr-x    5 root     root           360 Mar 18 09:47 dev
drwxr-xr-x   13 root     root          4096 Mar 18 09:47 etc
drwxr-xr-x    2 root     root          4096 Mar  2 16:20 home
drwxr-xr-x    5 root     root          4096 Mar  2 16:20 lib
......
......

containers are fast!
-------------------
below is the quick execution reveals that!


$ docker run alpine echo "hello from alpine"
hello from alpine

$ docker run alpine /bin/sh


to not exit, you need to  
$ docker run -it alpine /bin/sh 


$ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS               NAMES
36171a5da744        alpine              "/bin/sh"                5 minutes ago       Exited (0) 2 minutes ago                        fervent_newton
a6a9d46d0b2f        alpine             "echo 'hello from alp"    6 minutes ago       Exited (0) 6 minutes ago                        lonely_kilby
ff0a5c3750b9        alpine             "ls -l"                   8 minutes ago       Exited (0) 8 minutes ago                        elated_ramanujan
c317d0a9e3d2        hello-world         "/hello"                 34 seconds ago      Exited (0) 12 minutes ago   


more than one commmand/cmd in a container
-------------------------------------------

$ docker run -it alpine /bin/sh
/ # ls
bin      dev      etc      home     lib      linuxrc  media    mnt      proc     root     run      sbin     sys      tmp      usr      var
/ # uname -a
Linux 97916e8cb5dc 4.4.27-moby #1 SMP Wed Oct 26 14:01:48 UTC 2016 x86_64 Linux

----------

Images 
	docker inspect alpine
Containers 
	shares the kernel with other containers, and runs as an isolated process in user space on the host OS.
Docker daemon -
	 The background service running on the host that manages building, running and distributing Docker containers.
Docker client 
	- The command line tool that allows the user to interact with the Docker daemon.
Docker Hub 
	- A registry of Docker images. You can think of the registry as a directory of all available Docker images. You'll be using this later in this tutorial.


-------------------

run static site

	$ docker run -d dockersamples/static-site

$ docker ps
CONTAINER ID        IMAGE                  COMMAND                  CREATED             STATUS              PORTS               NAMES
a7a0e504ca3e        dockersamples/static-site   "/bin/sh -c 'cd /usr/"   28 seconds ago      Up 26 seconds       80/tcp, 443/tcp     stupefied_mahavira

$ docker stop a7a0e504ca3e
$ docker rm   a7a0e504ca3e

Now, let's launch a container in detached mode as shown below:
$ docker run --name static-site -e AUTHOR="Your Name" -d -P dockersamples/static-site
e61d12292d69556eabe2a44c16cbd54486b2527e2ce4f95438e504afb7b02810

 -d  will create a container with the process detached from our terminal
"  -P  will publish all the exposed container ports to random ports on the Docker host
"  -e  is how you pass environment variables to the container
"  --name  allows you to specify a container name
"  AUTHOR  is the environment variable name and  Your Name  is the value that you can pass

$ docker port static-site
443/tcp -> 0.0.0.0:32772
80/tcp -> 0.0.0.0:32773


 http://localhost:32773 
 
 
 $ docker-machine ip default
192.168.99.100

 http://192.168.99.100:32773 
 
 
 run second web server:
 ---------------------
 
 $ docker run --name static-site-2 -e AUTHOR="Your Name" -d -p 8888:80 dockersamples/static-site
 
 	http://192.168.99.100:8888
 	
 $ docker stop static-site
$ docker rm static-site

$ docker rm -f static-site-2

$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES


Docker Images
-------------
$ docker pull ubuntu
	=
$ docker pull ubuntu:latest


specific version pull

$ docker pull ubuntu:12.04

search docker hub
$ docker search 

```

Base images are images that have no parent images, usually images with an OS like ubuntu, alpine or debian.

Child images are images that build on base images and add additional functionality.


https://github.com/docker/labs/blob/master/beginner/chapters/webapps.md#231-create-a-python-flask-app-that-displays-random-cat-pix
