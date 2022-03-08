---
title: How to install and use docker on ubuntu 18.04
author:
  name: Lin Hong Zhou
  link: https://linhongzhou.github.io/
date: 2021-01-03 18:32:00 -0500
categories: [Operating]
tags: [docker, ubuntu]
---


In this tutorial, you’ll install and use Docker Community Edition (CE) on Ubuntu 18.04. You’ll install Docker itself, work with containers and images.

## Introduction
Docker is an application that simplifies the process of managing application processes in containers. Containers let you run your applications in resource-isolated processes. They’re similar to virtual machines, but containers are more portable, more resource-friendly, and more dependent on the host operating system.

## Prerequisites


To complete this guide, you will need access to an Ubuntu 18.04 server that has a non-root user with sudo privileges and a basic firewall configured.

When you are ready to begin, log in to your Ubuntu 18.04 server as your sudo user and continue below.

## Step 1 — Installing Docker

### Set up the repository
Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. 

1. Update the apt package index and install packages to allow apt to use a repository over HTTPS:


```bash
sudo apt update

sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

```

2. Add Docker’s official GPG key:

```
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

```


3. Use the following command to set up the stable repository. To add the nightly or test repository, add the word nightly or test (or both) after the word stable in the commands below.

```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

```


### Install Docker Engine
1. Update the apt package index, and install the latest version of Docker Engine and containerd

```
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io
```

2. Verify that Docker Engine is installed correctly by running the hello-world image.
```
sudo docker run hello-world
```

This command downloads a test image and runs it in a container. When the container runs, it prints a message and exits.



Docker should now be installed, the daemon started, and the process enabled to start on boot. Check that it’s running:

```bash
sudo systemctl status docker
```
    
## Step 2 — Using the Docker Command

Using docker consists of passing it a chain of options and commands followed by arguments. The syntax takes this form:
```bash
docker [option] [command] [arguments]
```

To view current version of docker, type:
```
docker -v
```

You’ll see output like this, although the version number for Docker may be different:
```
Docker version 20.10.12, build e91ed57
```


To view all available subcommands, type:
```
docker
```

As of Docker 20, the complete list of available subcommands includes:
```
Usage:  docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default "/home/topsee/.docker")
  -c, --context string     Name of the context to use to connect to the daemon (overrides DOCKER_HOST env var and default
                           context set with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level ("debug"|"info"|"warn"|"error"|"fatal") (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default "/home/topsee/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default "/home/topsee/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default "/home/topsee/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

Management Commands:
  app*        Docker App (Docker Inc., v0.9.1-beta3)
  builder     Manage builds
  buildx*     Docker Buildx (Docker Inc., v0.7.1-docker)
  config      Manage Docker configs
  container   Manage containers
  context     Manage contexts
  image       Manage images
  manifest    Manage Docker image manifests and manifest lists
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  scan*       Docker Scan (Docker Inc., v0.12.0)
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes

Run 'docker COMMAND --help' for more information on a command.
```

To view system-wide information about Docker, use:
```
docker info
```