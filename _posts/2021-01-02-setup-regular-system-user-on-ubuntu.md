---
title: How to set up a regular system user on Ubuntu 18.04
author:
  name: Lin Hong ZHOU
  link: http://linhongzhou.github.io
date: 2021-01-02 11:33:00 +0800
categories: [Operating]
tags: [ubuntu]
math: true
mermaid: true
# image:
#   src: /images/commons/devices-mockup.png
#   width: 800
#   height: 500
---

Newly installed servers typically have only a root account set up, and that is the account you’ll use to log into your server for the first time.

The root user is an administrative user that has very broad privileges. Because of the heightened privileges of the root account, you are discouraged from using it on a regular basis. This is because part of the power inherent to the root account is the ability to make very destructive changes, even by accident. For that reason, the recommended practice is to set up a regular system user and give this user sudo permissions, so that it may run administrative commands with certain limitations. In this tutorial, you’ll set up such a user.


## Step 1 — Logging in as Root

To get started, you’ll need to log into your server as the root user with the following command:


```
ssh root@your_server_ip
```


## Step 2 — Creating a New User

Once you are logged in as root, you can create a new user that will be your regular system user from now on.

The following command creates a new user called lin, but you should replace it with a username of your choice:

```
adduser lin

```


## Step 3 — Granting Administrative Privileges

You have now a new user account with regular privileges. Sometimes, however, you’ll need to perform administrative tasks, like managing servers, editing configuration files, or restarting a server.

To avoid having to log out of your regular user and log back in as the root account, you can set up what are known as “superuser” or root privileges for your regular account. This will allow your regular user to run commands with administrative privileges by prefixing each command with the word sudo.

To add these privileges to you new user, you need to add the new user to the sudo group. By default on Ubuntu 18.04, users who belong to the sudo group are allowed to use the sudo command.

The following command will modify the default user settings, including the sudo group in the list of groups a user already belongs to. Pay attention to the -a argument, which stands for append. Without this option, the current groups a user is linked to would be replaced by sudo, which would cause unexpected consequences. The -G argument tells usermod to change a user’s group settings.

As root, run this command to add your new user to the sudo group (replace the highlighted word with your new user):

```
usermod -aG sudo lin
```

Your system user is now set up. 

