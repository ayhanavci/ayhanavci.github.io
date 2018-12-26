---
title: "Dockerizing Python on Linux"
excerpt_separator: "<!--more-->"
description: "Dockerizing Python project on a Posix host (by Ayhan Avcı)"
date: "2018-07-29"
excerpt: "Dockerizing Python web project on a Posix host. Source and how-to included with a brief information on concepts."
header:
    og_image: /assets/images/simplicity3.png
categories:
  - Projects
tags:
  - Python
  - Web
  - Docker
  - PostgreSQL
  - Linux
---

{% include toc title="Contents" icon="list-ul" %}

## Introduction

How do you ship your software product? [Docker](https://www.docker.com/) aims to solve shipment issues by isolating software environments without having to install a whole operating system from scratch, aiming to solve eternal conflict between system administrators and software developers: "But it works on my machine!" with as little resource consumption as possible. You dockerize your production environment, and if it works on your development environment, it is going to work on production.

As a bonus, it also works good for isolating each project on your development environment thanks to the nature of Docker Compose.

In this post, I am going to Dockerize my own project that I have previously written. You can find the [source on github](https://github.com/ayhanavci/LainSimpleGrades).

![Docker]({{ site.url }}{{ site.baseurl }}/assets/images/dockerlogo.png){:height="150px" width="180px"}
![Linux]({{ site.url }}{{ site.baseurl }}/assets/images/linuxlogo.png){:height="218px" width="197px"}

I'm saying Linux, but all of the below works on OS X and Windows too. 

## Goals

1. Dockerize the Grades Web application previously described in [Web with Python]({% post_url 2018-07-28-Web-With-Python %})
2. Dockerize PostGreSQL
3. Bind the Web and Database containers.
4. Automate everything (downloading Docker images, , installing software, creating containers, binding them, creating database & tables, running web application) into single line command.
5. Everything, from the project to Docker, should all be cross platform and use free software.
6. Minimalism, Full Encapsulation & Documentation

## How It Works

It is best to read [official documentation](https://docs.docker.com/) to learn about Docker. But I'm going to give a brief explanation. Here is what you do to dockerize your software:

1. Install [Docker Community Edition](https://www.docker.com/community-edition)
2. Write a ```Dockerfile```
3. ```Docker build PATH``` as explained in [Docker Build](https://docs.docker.com/engine/reference/commandline/build/).
4. use docker-compose for multi-container applications

Below is my version of a ```Dockerfile```. There is a lot you can do in a Dockerfile, I am keeping it minimal.

### Dockerfile

```
FROM python:3.7
ENV PYTHONUNBUFFERED 1
RUN mkdir /lainsimplegrades
WORKDIR /lainsimplegrades
ADD requirements.txt /lainsimplegrades/
RUN pip install -r requirements.txt
ADD . /lainsimplegrades/
EXPOSE 8000
```

Running docker build on project directory uses this configuration file. It downloads python environment and copies project files inside the container. It then installs whatever dependency is included in requirements.txt and exposes port 8000 to outside world.

### requirements.txt

```
Django==2.0
psycopg2
```

First one installs Django framework, second one installs PostgreSQL adapter required for Python. 

This produces a Docker image which you can ship. But this still needs all the configuration for production. My goal was to automate everything. So I used [Docker Compose](https://docs.docker.com/compose/overview/) which is a tool used to define and run multi-container applications with Docker. My goal was running Python Web + PostgreSQL containers together. So that fits in multi-container criteria.

For that purpose, you write a ```docker-compose.yml``` file. Explaining details is out of scope of this post and it is best to follow [official docs](https://docs.docker.com/compose/overview/). 

### docker-compose.yml

```yml
version: '3'

services:
  db:
    image: postgres
    restart: always
    environment:
      POSTGRES_USER: ayhanavci
      POSTGRES_PASSWORD: demopass
      POSTGRES_DB: lainsimplegrades
  web:
    build: .
    command: sh -c './wait-for-it.sh db:5432 -t 10 -- ./init.sh '
    image: ayhanavci/lainsimplegrades 
    volumes:
      - .:/lainsimplegrades
    ports:
      - "8001:8000"
    depends_on:
      - db

```

Here is the breakdown;

services:
* We define two services. ```db``` and ```web```. web service depends on db service.

DB:
* Service ```db``` uses postgres image. Initially this doesn't exist. So ```docker-compose``` is going to download this one from Docker Hub. Here is what gets downloaded: https://hub.docker.com/_/postgres/
* We define some of the environment variables under the environment as described on official Postgres hub.

Web:
* Builds using Dockerfile, which in turn downloads and installs Python, Django and Postgres adapter. The project files are also copied inside. ayhanavci/lainsimplegrades Docker image is created. (```docker images``` bash command for a list of images)
* Python image for this service is downloaded from here https://hub.docker.com/_/python/
* volumes bind the /lainsimplegrades folder inside the container onto current directory (.). So when we make any change, it is reflected inside.
* ports connects container's port 8000 into host's port 8001. I made it this way so that the difference is clear. Typically you bind the same host port.
* command runs a bash script that automates everything else. These scripts will be explained below.

### Bash Scripts

Docker Compose creates and runs both the Python web server and the Postgres database. But it does not guarantee that Database server is up and running before the web server. But we want to make sure DB is running before web. The script does that and it also creates the necessary tables.

Here is the breakdown of the bash command run at docker-compose.yml

```bash
sh -c './wait-for-it.sh db:5432 -t 10 -- ./init.sh '
```

* sh -c means "Read commands from the command_string operand instead of from the standard input". So it executes the part './wait-for-it.sh db:5432 -t 10 -- ./init.sh ' on shell
* I have used the popular wait-for-it.sh shell script. It is commonly used to wait for a port to be ready and then do whatever you want it to do. [Here is the github page](https://github.com/vishnubob/wait-for-it)
* The parameters are db:5432 means you want it to wait for the db container's 5432 port. Which is the Postgres Docker container's database port. You make it wait for 10 seconds and if it succeeds, execute init.sh.
* init.sh 

```bash
#!/bin/sh

echo "Make Migrations"
python3 ./LainSimpleGrades/manage.py makemigrations Grades
echo "Migrate"
python3 ./LainSimpleGrades/manage.py migrate
echo "Run Server"
python3 ./LainSimpleGrades/manage.py runserver 0.0.0.0:8000
```

These are Django commands to create database, create tables and host the web application on port 8000.

As you can see port 8000 is the port of the docker container. Docker compose binds this port into port 8001 of the host machine as instructed on docker-compose.yml. These port numbers are just random. You can use whatever you like.

## Docker Hub

Where are all those images downloaded from? Most major companies support Docker and upload their dockerized products to [Docker Hub](https://hub.docker.com/). For instance here is [Python](https://hub.docker.com/_/python/), and here is [Postgres](https://hub.docker.com/_/postgres/), here is [Ubuntu](https://hub.docker.com/_/ubuntu/). 

Simple Grades demo application is also on Docker automated builds. Whenever I push updates to my code, Docker automatically creates a new image on site. [Ayhan Avcı on Docker](https://hub.docker.com/r/ayhanavci/lainsimplegrades/)

## Common Docker Commands

[Here is the official list](https://docs.docker.com/engine/reference/commandline/docker/)

Some important ones;

Get Docker version

docker version
{: .notice}

Lists all Docker images

docker images
{: .notice}

Lists all Docker containers

docker ps -a
{: .notice}

Build an image from docker file

docker build [Path]
{: .notice}

Build an image from github. (ie: https://github.com/docker/rootfs.git#container:docker)

docker build [Github Path]#container:docker 
{: .notice}

Run a docker-compose.yml file (add -d parameter to run in background)

docker-compose up
{: .notice}

## On Ubuntu Host

This works both on OS X & Ubuntu without any issues. Should also work on any Debian variant Linux distro but I haven't tested. Use sudo whenever necessary. 

* Install Docker Community Edition (Free one)

```bash
sudo apt-get update
sudo apt-get install docker-ce
```
On OS X, use brew. Test the installation by docker version

* Download the source code & required docker files for automation;

```bash
git clone https://github.com/ayhanavci/LainSimpleGrades.git
```

* Run the automation. (docker-compose up with -d parameter if you would like to run as a daemon)

```bash
cd LainSimpleGrades
docker-compose up
```

Here is how it looks on my Ubuntu machine. Currently I am serving the dockerized application at [http://grades.lain.run/](http://grades.lain.run/) But since this is my sandbox machine, there is no guarantee I will keep it running.

![Docker Ubuntu]({{ site.url }}{{ site.baseurl }}/assets/images/dockerubuntu.png){:height="360px" width="576px"}

[Click here to zoom image]({{ site.url }}{{ site.baseurl }}/assets/images//dockerubuntu.png){:target="_blank"}

lainsimplegrades image is derived from Python image, which is a Linux image with Python installed. The image also has Django, source codes and Postgres connection adapter installed on it. 

## Source Code

[Source on Github](https://github.com/ayhanavci/LainSimpleGrades). Project files and docker files for fully automated Dockerized deployment

## Live Demo

Currently I am Running my dockerized project at [http://grades.lain.run/](http://grades.lain.run/). But this is my sandbox server, so I cannot guarantee it will stay up all the time.

## Conclusion

Docker is a great tool both for isolated development environment and for deployment purposes. As of 2018, every single major hosting company support Docker. And all software providers have created their official Docker files along with documentation. So Docker is the future.

As for me, I have achieved my goal of fully automating the deployment. Both the code and the Docker automation I have provided on github should run on every hosting platform that Docker can be installed without a single change to the codes.