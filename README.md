# What is Docker ?

“Docker is an open platform for developers and sysadmins to build, ship, and run distributed
applications. Consisting of Docker Engine, a portable, lightweight runtime and packaging tool, and
Docker Hub, a cloud service for sharing applications and automating workflows, Docker enables
apps to be quickly assembled from components and eliminates the friction between development,
QA, and production environments. As a result, IT can ship faster and run the same app, unchanged,
on laptops, data center VMs, and any cloud.”

# Contents

1. Container vs VM 
2. Docker System
3. Images / Working with File
4. Containers / Working with Containers
5. Registries and Repositories / Working with Registries 
6. Docker Networking / Working with Networking

# Container vs VM

![Alt text](https://github.com/maheshrajanna/Docker/blob/master/VM.png?raw=true "Optional Title")


# Docker System

![Alt text](https://github.com/maheshrajanna/Docker/blob/master/System.png?raw=true "Optional Title")


# Images

* Images are static frozen time containers where we launch containers from.  Images may be single or comprised of 2 or more layered images.

# Working with Images

![Alt text](https://github.com/maheshrajanna/Docker/blob/master/images.png?raw=true "Optional Title")

# File command description

* **FROM**	image|scratch base image for the build
* **MAINTAINER**email name of the mainainer (metadata)
* **COPY** 	path dst copy path from the context into the container at location dst
* **ADD** 	src dst same as COPY but untar archives and accepts http urls
* **RUN** 	args. . . run an arbitrary command inside the container
* **USER** 	name set the default username
* **WORKDIR** 	path set the default working directory
* **CMD** 	args. . . set the default command
* **ENV** 	name value set an environment variable


#Docker File

![Alt text](https://github.com/maheshrajanna/Docker/blob/master/file.png?raw=true "Optional Title")


Docker file creation! 

#vi Dockerfile

```
# Simple web server
	FROM ubuntu:15.04
	RUN apt-get update
	RUN apt-get install -y apache2
	RUN apt-get install -y apache2-utils
	RUN apt-get clean
	EXPOSE 80
CMD ["apache2ctl","-D","FOREGROUND"]

```

**Build image from docker file**

```
# docker build -t new-web:0.1 .    [Building a docker image from file "." represents the docker file in same location]
# docker history containerID       [Checking last build detials]

```

# Containers

# What is container?
A container image is a lightweight, stand-alone, executable package of a piece of software that includes everything needed to run it: code, runtime, system tools, system libraries, settings.
LIGHTWEIGHT
STANDARD
SECURE

Working with containers

![Alt text](https://github.com/maheshrajanna/Docker/blob/master/Containers.png?raw=true "Optional Title")


### Container commands

```
# docker run -it --name=myadmin myadmin /bin/bash  (docker run interactivemode definingName Image /bin/bash )
						
# docker run –it -d myadmin (-d running in detached mode)
# docker stop myadmin  
# docker start myadmin

# docker attach myadmin (attaching a detached container) (Attache PID 1 inside the container) 									(you can attacheh containers only created with /bin/bash)

  CTL+P+Q  (Detach container)

# docker exce –it myadmin bash (connecting container in bash)

# docker ps / docker ps -a  (listing container running/exited)

# docker ps –l (Shows last container we ran)

# ls /var/lib/docker/containers/containerID  (container details Path)

# docker inspect myadmin (getting container detailed information)

# docker rm myadmin (rmi for images) (Deleting container and images)

# docker top myadmin (checking running services without outside container PID will be differed form inside container)

# ps –ef  (Chechinf PID inside container)

UID        PID  PPID  C STIME TTY          TIME CMD
mysql        1     0  0 Apr27 ?        00:00:04 mysqld
root        81     0  0 00:00 ?        00:00:00 bash
```

**PID**

![Alt text](https://github.com/maheshrajanna/Docker/blob/master/PID.png?raw=true "Optional Title")


# Registries and Repositories


![Alt text](https://github.com/maheshrajanna/Docker/blob/master/Reg.png?raw=true "Optional Title")


* Docker Hub
* Docker Hub is a registry (repository) where we pull images, public by default. In registry there will be number of repos. We can maintain private registry at docker hub. 

# Working with Registries

![Alt text](https://github.com/maheshrajanna/Docker/blob/master/repo.png?raw=true "Optional Title")

# Working with Docker Hub

```
# docker images

  docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
# docker tag imageID maheshraj/smart_private:1.0    (where as "maheshraj/smart_private" is repository we have creted in docker HUB)

# docker login
# docker push maheshraj/smart_private:1.0  (Pushing the tagged image to docker HUB with 1:0 version)
```
```
# docker rmi maheshraj/smart_private:1.0 (removing tagged image from local host)
# docker pull maheshraj/smart_private:1.0 (Pulling the images from our docker hub)
```


# Private Docker Registries

  We can creted our own private registry in our local host by creating "registry" container. 
```
#docker run -d -p 5000:5000 --restart=always --name registry registry:2  (exposing 5000 port to local host from container)
									 (--restart=always Always starts when host starts) 
									 
#docker pull Ubuntu 
#docker tag imageID localhost.localdomain:5000/Ubuntu (where as "localhost.localdomain:5000" is repository we have creted in local Repo)
```
```
#docker push localhost.localdomain:5000/Ubuntu 	(Pushing to private Registry)
#docker rmi 6a2f32de169d (remove image)
#docker pull localhost.localdomain:5000/Ubuntu	(Pulling from private Registry)
```


# Docker Networking


![Alt text](https://github.com/maheshrajanna/Docker/blob/master/1.png?raw=true "Optional Title")


## Netwok commands

```sh
# ip a  [ check the bridge interface/switch from Host ]
# yum install bridge-utils
# brctl show docker0 or brctl show (List all interface details)
# ping 8.8.8.8 and #ip a (inside container check)
# docker inspect de2d2ea90024 
# ls -la /var/lib/docker/containers/de2d2e/ (resolve.conf is a copy of host file)
# docker run –dns=8.8.4.4 –name=testdns image (then inspect dns)

```

### Working with Ports

* [EXPOSE]   -  Document where a service is available, but not create any mapping to the host
* [--expose] -	Expose a port at runtime, but not create any mapping to the host
* [-p	]    -	Create a port mapping rule like -p ip:hostPort:containerPort. containerPort is required. If no hostPort is                               specified, Docker will automatically allocate one.
* [-P	]    -	Map a dynamically allocated host port to all container ports that have been exposed by the Dockerfile
* [--link]   -    Create a link between a consumer and service container, like --link name:alias. This will create a set of environment                   variables and add entries into the consumer container's /etc/hosts file. You must also expose or publish ports.
   
   “All published (-p or -P) ports are exposed, but not all exposed (EXPOSE or --expose) ports are published”



# Port expose

```sh
#docker run -p 8080:80 --name myadmin -d phpmyadmin
#docker run -p 8080:80/upd --name myadmin -d phpmyadmin
#docker run -p 192.168.2.5 8080:80 --name myadmin -d phpmyadmin
--expose=1234
--expose=2000-3000
#docker port myadmina1 (Viewing port)
# docker run –P –name myadmin –d phpadmin (All exposed ports are mapped to high number host ports)
```

# Linking containers

```sh
# docker run -it --name MyApache -v /tmp/httpd:/var/www/html -p 80:80 -d php:7.0-apache
apt-get update
apt-get install mysql-client
#docker run --name mysql1 -v /opt/mysql1:/var/lib/mysql --link=MyApache:Ali-MyApache -e MYSQL_ROOT_PASSWORD=Superpassword1 -d mysql
(connect to mysql container from apach container, without exposing port)
#docker inspect mysql | grep Links
```


# Understanding Docker Compose

On this page you build a simple Python web application running on Docker Compose. The application uses the Flask framework and maintains a hit counter in Redis. While the sample uses Python, the concepts demonstrated here should be understandable even if you’re not familiar with it.

Step 1: Setup

1. Create a directory for the project:

```
$ mkdir composetest
$ cd composetest
```

2. Create a file called app.py in your project directory and paste this in:

```
from flask import Flask
from redis import Redis

app = Flask(__name__)
redis = Redis(host='redis', port=6379)

@app.route('/')
def hello():
    count = redis.incr('hits')
    return 'Hello World! I have been seen {} times.\n'.format(count)

if __name__ == "__main__":
    app.run(host="0.0.0.0", debug=True)

```

In this example, redis is the hostname of the redis container on the application’s network. We use the default port for Redis, 6379.

3. Create another file called requirements.txt in your project directory and paste this in:

```
flask
redis
```
