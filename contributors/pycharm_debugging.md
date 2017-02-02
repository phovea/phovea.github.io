---
layout: documentation
title:  PyCharm Debugging
order: 2
---

# PyCharm debugging

In order to debug the server using PyCharm you need to do two things:

## Prepare environment

Restart docker with the debug mixin

```
./docker-compose.debug up
```

## Create new remote Python interpreter

1. Within PyCharm go to Settings -> Project Interpreter -> Cogs symbol -> Add Remote...
1. Select SSH-Credentials and use the following values:
 * Host: `localhost`
 * Port: `2222`
 * User name: `root`
 * Password: `docker`
 * Python interpreter path: `/usr/local/bin/python` **!this is different from the default**
1. PyCharm should now be able to connect to the docker container and upload some helper files
1. Create default Path mappings
 1. Select project directory and specify it is located at `/phovea`
 1. In the end the overview page should state: `<Project root>->/phovea`


## Create and run the server using PyCharm

TODO
