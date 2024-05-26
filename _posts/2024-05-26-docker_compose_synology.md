---
layout: post
title:  "Synology Docker compose (Project): Container stopped immediately after starting"
---

One problem that took ne some time when using docker-compose ("Project") on my Synology NAS. The container stopped immediately after starting. And unfortunately there were no further and helpful error messages

The solution was to add the instruction **tty: true** to allocate a pseudo terminal.
The next problem was after opening a terminal that no input was possible. Therefore, it must be added **stdin_open: true**.

Here is a sample:

```
services:
    myname:
    image: myimage
    container_name: targetname
    tty: true      
    stdin_open: true
```




