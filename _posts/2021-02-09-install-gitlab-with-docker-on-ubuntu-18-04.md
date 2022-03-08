---
title: How to install gitlab with docker on Ubuntu 18.04
author:
  name: Lin Hong Zhou
  link: https://linhongzhou.github.io/
date: 2021-02-09 20:55:00 +0800
categories: [Operating]
tags: [docker, ubuntu, gitlab]
pin: false
---


In this tutorial, you will install and configure GitLab, a popular open-source application that primarily hosts Git repositories, on Ubuntu 18.04.

## Prerequisites

To follow this tutorial, you will need the following:
* Ubuntu 18.04 Server
* Min RAM memory 4GB - for better performance, use 8GB
* Root privileges 
* Docker is required(see [How to install and use docker on ubuntu 18.04](/posts/install-and-use-docker-on-ubuntu-18-04/))


## Installation

### Creating gitlab home

```
mkdir -p /data/gitlab
export GITLAB_HOME=/data/gitlab

```

### Installing gitlab by docker
```bash
sudo docker run --detach \
  --publish 8008:80 \
  --publish 8022:22 \
  --name gitlab \
  --restart always \
  --volume $GITLAB_HOME/config:/etc/gitlab \
  --volume $GITLAB_HOME/logs:/var/log/gitlab \
  --volume $GITLAB_HOME/data:/var/opt/gitlab \
  --shm-size 2048m \
  gitlab/gitlab-ce:latest
```


## configuration

you can change the configurations in the /data/gitlab, or enter the docker container .


```
sudo docker exec -it gitlab /bin/bash
```

### external_host

edit the file /etc/gitlab/gitlab.rb:

```
external_url 'http://192.168.1.188'

```

### ssh port

edit the file /etc/gitlab/gitlab.rb

```
vim /etc/gitlab/gitlab.rb
```
Find the line below and change the ssh port to 8022:
```
gitlab_rails['gitlab_shell_ssh_port'] = 8022
```


### ssh host

edit the file:
/var/opt/gitlab/gitlab-rails/etc

```
vim /var/opt/gitlab/gitlab-rails/etc
```

find the host name ,something like 'e8d66a682bbe', and change all of those to your real ip or server name.

```
vim /opt/gitlab/embedded/service/gitlab-rails/config/gitlab.yml
  ## GitLab settings
  gitlab:
    ## Web server settings (note: host is the FQDN, do not include http://)
    # host: e8d66a682bbe
    host: 192.168.1.188
    port: 80
    https: false
```

### gitlab-ctl reconfigure
run reconfigure command in the container's bash terminal:

```
gitlab-ctl reconfigure
```

### restart gitlab container

```
sudo docker restart gitlab

```







