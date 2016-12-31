---
layout: documentation
title:  PyCharm Debugging
order: 2
---

# PyCharm Debugging

In order to debug the server using PyCharm you need to do two things: 

## prepare environment

1. patch `docker-compose.yml`

```
service:
  api:
    ports:
      - '9000:80'
      - '2222:22'
    command: /usr/sbin/sshd -D
    volumes:
      - '.:/phovea'
```

add the new port entry and the new command entry. 
This will say docker-compose not to launch the server by default but a ssh server to which PyCharm can connect to. 
In addition, the ssh port is mapped to the port 2222. 

1. restart service
 ```
 docker-compose restart
 ```
 
## create new remote python interpreter

1. within PyCharm go to settings -> Project Interpreter -> Cogs symbol -> Add Remote...
1. select SSH-Credentials and use the following values: 
 * Host: `localhost`
 * Port: `2222`
 * User name: `root`
 * Password: `docker`
 * Python interpreter path: `/usr/local/bin/python` **!this is different from the default**
1. PyCharm should now be able to connect to the docker container and upload some helper files


## create and run the server using PyCharm

TODO
