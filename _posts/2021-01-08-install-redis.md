---
title: How to install Redis on Ubuntu 18
author:
  name: Lin Hong ZHOU
  link: http://linhongzhou.github.io
date: 2021-01-08 14:10:00 +0800
categories: [Operating]
tags: [ubuntu, redis]
mermaid: true
---

## Introduction

Redis is an in-memory key-value store known for its flexibility, performance, and wide language support. This tutorial demonstrates how to install, configure, and secure Redis on an Ubuntu 18.04 server.

## Prerequisites

To complete this guide, you will need access to an Ubuntu 18.04 server that has a non-root user with sudo privileges and a basic firewall configured.

When you are ready to begin, log in to your Ubuntu 18.04 server as your sudo user and continue below.

## Step 1 — Installing and Configuring Redis

In order to get the latest version of Redis, we will use apt to install it from the official Ubuntu repositories.

First, update your local apt package cache if you haven’t done so recently:


```
sudo apt update

```
Then, install Redis by typing:

```
sudo apt install redis-server
```
This will download and install Redis and its dependencies. 

Following this, there is one important configuration change to make in the Redis configuration file, which was generated automatically during the installation.

Open this file with your preferred text editor:
```
sudo vim /etc/redis/redis.conf
```

Inside the file, find the supervised directive. This directive allows you to declare an init system to manage Redis as a service, providing you with more control over its operation. The supervised directive is set to no by default. Since you are running Ubuntu, which uses the systemd init system, change this to systemd:
```
# If you run Redis from upstart or systemd, Redis can interact with your
# supervision tree. Options:
#   supervised no      - no supervision interaction
#   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
#   supervised systemd - signal systemd by writing READY=1 to $NOTIFY_SOCKET
#   supervised auto    - detect upstart or systemd method based on
#                        UPSTART_JOB or NOTIFY_SOCKET environment variables
# Note: these supervision methods only signal "process is ready."
#       They do not enable continuous liveness pings back to your supervisor.
supervised systemd

```

That’s the only change you need to make to the Redis configuration file at this point, so save and close it when you are finished. Then, restart the Redis service to reflect the changes you made to the configuration file:

```
sudo systemctl restart redis.service
```

## Step 2 — Testing Redis

As with any newly-installed software, it’s a good idea to ensure that Redis is functioning as expected before making any further changes to its configuration. We will go over a handful of ways to check that Redis is working correctly in this step.

Start by checking that the Redis service is running:

```
sudo systemctl status redis

```

If it is running without any errors, this command will produce output similar to the following:

```
● redis-server.service - Advanced key-value store
   Loaded: loaded (/lib/systemd/system/redis-server.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2022-03-07 18:25:56 CST; 16h ago
     Docs: http://redis.io/documentation,
           man:redis-server(1)
  Process: 1451 ExecStart=/usr/bin/redis-server /etc/redis/redis.conf (code=exited, status=0/SUCCESS)
 Main PID: 1529 (redis-server)
    Tasks: 4 (limit: 8601)
   CGroup: /system.slice/redis-server.service
           └─1529 /usr/bin/redis-server 127.0.0.1:6379


```

Here, you can see that Redis is running and is already enabled, meaning that it is set to start up every time the server boots.

>Note: This setting is desirable for many common use cases of Redis. If, however, you prefer to start up Redis manually every time your server boots, you can configure this with the following command:
```
sudo systemctl disable redis
```
{: .prompt-info }


To test that Redis is functioning correctly, connect to the server using the command-line client:
```
redis-cli
```

In the prompt that follows, test connectivity with the ping command:
```
127.0.0.1:6379> ping
```

```
Output
PONG
```
This output confirms that the server connection is still alive. Next, check that you’re able to set keys by running:
set test "It's working!"

```
Output
OK
```
Retrieve the value by typing:
```
get test
```

```
Output
"It's working!"
```

As a final test, we will check whether Redis is able to persist data even after it’s been stopped or restarted. To do this, first restart the Redis instance:
```
get test
```

The value of your key should still be accessible:
```
Output
"It's working!"
```

Exit out into the shell again when you are finished:
```
127.0.0.1:6379> exit
```


Step 3 — Configuring a Redis Password

Configuring a Redis password enables one of its two built-in security features — the auth command, which requires clients to authenticate to access the database. The password is configured directly in Redis’s configuration file, /etc/redis/redis.conf, so open that file again with your preferred editor:

```
sudo nano /etc/redis/redis.conf
```

Scroll to the SECURITY section and look for a commented directive that reads:
```
requirepass your_redis_password
```
Uncomment it by removing the #, and change foobared to a secure password.

After setting the password, save and close the file, then restart Redis:
```
sudo systemctl restart redis.service
```

To test that the password works, access the Redis command line:
```
redis-cli
```
The following shows a sequence of commands used to test whether the Redis password works. The first command tries to set a key to a value before authentication:
```
127.0.0.1:6379> set key1 0
(error) NOAUTH Authentication required.
```

That won’t work because you didn’t authenticate, so Redis returns an error:
```
(error) NOAUTH Authentication required.
```

The next command authenticates with the password specified in the Redis configuration file:

```
auth your_redis_password
```
Redis acknowledges:
```
Output
OK
```

After that, running the previous command again will succeed:
```
127.0.0.1:6379> set key1 0
OK
```


## Conclusion
In this tutorial, you installed and configured Redis, validated that your Redis installation is functioning correctly, and used its built-in security features to make it less vulnerable to attacks from malicious actors.

## Reference


1. [How To Install and Secure Redis on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-redis-on-ubuntu-18-04)

