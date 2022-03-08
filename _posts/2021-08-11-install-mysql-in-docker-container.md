---
title: How to install Mysql 5.7 in docker container on Ubuntu 18.04
author:
  name: Lin Hong Zhou
  link: https://linhongzhou.github.io/
date: 2021-08-11 00:34:00 +0800
categories: [Operating]
tags: [docker, mysql, ubuntu]
---

Installing MySQL in docker container is an easy process which can be done by pulling a docker image, deploying the MySQL container and connecting to the MySQL Docker Container




## Step 1 - pull the docker image
The first step is to pull the docker image. Here we are downloading the 5.7 version. However, if you wish to install a particular version then you can replace the latest tag with the version number.

```
sudo docker pull  mysql:5.7
```

## Step 2 - verify your image
Verify if your image is locally stored or not by running the below command.
```
docker images
```

If everything is ok,you will see the message below:
```
mysql                5.7       8b94b71dcc1e   4 days ago      448MB


```

## Step 3 - Prepare directory for mounting

First, create conf.d directory on the host machine:

```
mkdir -p /data/mysql/conf.d
```
The conf.d directory will be mounting to /etc/mysql/conf.d in container.

Second, create mysql-data directory on the host machine:
```
mkdir -p /data/mysql/mysql-data
```

The mysql-data directory will be mounting to /var/lib/mysql in container.


## Step 4 - Run the mysql server container
```
sudo docker run --detach \
  --env "MYSQL_ROOT_PASSWORD=123456" \
  --publish 3306:3306 \
  --name mysql-server \
  --restart always \
  --volume /data/mysql/conf.d:/etc/mysql/conf.d \
  --volume /data/mysql/mysql-data:/var/lib/mysql \
 mysql:5.7

```


## Step 5 - Install mysql client

```
 apt-get install mysql-client
```




## Step 6 - Connect to Mysql server

connect in container:
```
sudo docker exec -it mysql-server mysql -h localhost -u root -p
```


connect in host machine:
```
mysql -h localhost -u root -p
```

connect in remote client:

```
mysql -h 192.168.1.188 -u root -p
```