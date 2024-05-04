---
layout: post
title:  "Microsoft Copilot: Dockerfile commands for SSH keybased authentication"
---

**KI and Microsoft Copilot** can be a remarkable help, if you can describe in sufficient detail what is required.
The emphasis is on sufficiently precise. And you should be able to judge the quality of the result.

At the moment I am adapting my working methods to Copliot possibilities. The last task with implementation via Copliot. 

#### Instruction Copilot:
```
Create a Dockerfile with SSH keybased identification with the following requirements
* create an image based on the current Ubuntu version
* the public key is id_rsa.pub 
* the user is username with the password username
```

The result worked out of the box. Fine.

```
# Use the official Ubuntu base image
FROM ubuntu:latest

# Install necessary packages (including OpenSSH server)
RUN apt-get update && \
    apt-get install -y openssh-server

# Create a new user named 'username' with password 'username'
RUN useradd -m -d /home/username -s /bin/bash username && \
    echo 'username:username' | chpasswd

# Copy the public key (id_rsa.pub) into the user's home directory
COPY id_rsa.pub /home/username/.ssh/authorized_keys

# Set proper permissions for the SSH directory and authorized_keys file
RUN chmod 700 /home/username/.ssh && \
    chmod 600 /home/username/.ssh/authorized_keys && \
    chown -R username:username /home/username/.ssh

# Expose SSH port (optional, but useful for remote access)
EXPOSE 22

# Start the SSH server
CMD ["/usr/sbin/sshd", "-D"]

```
