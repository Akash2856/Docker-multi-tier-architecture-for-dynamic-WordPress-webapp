# Docker Use Case Project

## Overview
This project showcases a practical use case using Docker technology. Follow these steps to master Docker's storage, networking, container integration, and linking capabilities.

## Use Case
The primary goal is to create a dynamic app/website using the WordPress framework, with MySQL as the backend database.

### Understanding Dependency
WordPress relies on MySQL for data storage. It's vital to launch the MySQL container before deploying the WordPress container.

### Multi-Tier Architecture
This setup comprises two distinct components: the MySQL container for data storage and the WordPress container for the website/application. They are interconnected, forming a multi-tier architecture.

### Getting Started
To begin:

**1. Install the MySQL image to launch a MySQL database container.**
  ```bash
  docker run -dit --name db -e MYSQL_ROOT_PASSWORD=redhat -e MYSQL_DATABASE=mydb -e MYSQL_USER=akash -e MYSQL_PASSWORD=akashdb -v /data:/var/lib/mysql mysql
  ```
**2. Install the WordPress image and use it to start a container hosting the WordPress application.**
  ```bash
  docker run -dit –name mywp -p 8080:80 wordpress:latest
  ```
![image](https://github.com/Akash2856/Docker-multi-tier-architecture-for-dynamic-WordPress-webapp/assets/49570016/7f4c30e9-447e-4f29-996d-8ed55a0f2c04)

![image](https://github.com/Akash2856/Docker-multi-tier-architecture-for-dynamic-WordPress-webapp/assets/49570016/d46b19e1-b8f3-4da3-b149-9a9e1ce179bb)

![image](https://github.com/Akash2856/Docker-multi-tier-architecture-for-dynamic-WordPress-webapp/assets/49570016/8baf1801-b9ca-4940-be8e-b8fb304d2cb2)


If someone restart the DBcontainer then container ip got change and wordpress unable to connect with mysql container. 

Every container has unique name in Docker.Docker provide the facility to connect to container by it's name.
  ```shell
  docker run -dit --name os2 centos:7
  docker run -dit --name os1 --link os2 ubuntu:14.04
  ```
**container os1 is link to container os2**
Inbehine name linking is also use IP of both container, If IP change then both container can't be connect togethor by it's name.
  ```bash
  docker network ls
  ```
Above command will give the list of all network present in docker. 
The given command shows a list of all networks in Docker. Typically, when you create a container, it's placed in a default network called "bridge". However, this default network doesn't let you connect containers using their names, even if their IP addresses change.

If you want two containers to easily connect to each other using their names, even if their IP addresses change, you need to create a custom network. This feature isn't available on the default network(Bridge Network). 
  ```shell 
  docker network create --driver bridge --subnet 10.1.2.0/24 name_of_network
  ```
Again launch container

```bash
 docker run -dit --name db --network lwnet -e MYSQL_ROOT_PASSWORD=redhat -e \
> MYSQL_DATABASE=mydb -e MYSQL_USER=akash -e MYSQL_PASSWORD=akashdb -v /data:/var/lib/mysql mysql
```
```bash
docker run -dit –name -network lwnet mywp -p 8080:80 wordpress:latest
```


