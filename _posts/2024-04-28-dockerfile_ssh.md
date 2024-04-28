---
layout: post
title:  "Dockerfile commands for SSH keybased authentication (Linux Container)"
---

My goal is to create a Docker container for software development that requires no further configuration. One requirement is to be able to access the container directly via SSH. There are a number of instructions on the Internet, but none of them worked for me out of the box. Here is my solution.


```
# Part of the Dockerfile
# The public key is available in the build directory
# Testet with Ubuntu container

ARG USER=username
ARG PASS="userpassword"
RUN useradd -m -s /bin/bash $USER && echo "$USER:$PASS" | chpasswd

RUN mkdir /home/$USER/.ssh/
RUN chmod 700 /home/$USER/.ssh

COPY id_rsa.pub /home/$USER/.ssh/authorized_keys
RUN chmod 600 /home/$USER/.ssh/authorized_keys
RUN chown -R $USER:$USER /home/$USER/.ssh

```
