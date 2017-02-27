---
layout: documentation
title:  PyCharm Debugging
order: 2
---

# PyCharm debugging

In order to debug the server using PyCharm you need to do two things:

1. [Prepare environment](#prepare-environment)
2. [Create new remote Python interpreter](#create-remote-python-interpreter)


<a id="prepare-environment"></a>
## Prepare environment

Restart Docker with the debug mixin

```
./docker-compose-debug up
```

Restart Docker in the *normal-mode* to exit the debug-mode:

```
docker-compose up
```

<a id="create-remote-python-interpreter"></a>
## Create new remote Python interpreter

1. Within PyCharm go to Settings -> Project Interpreter -> Cogs symbol -> Add Remote...
1. Select SSH-Credentials and use the following values:
 * Host: `localhost`
 * Port: `2222`
 * User name: `root`
 * Password: `docker`
 * Python interpreter path: `/usr/local/bin/python` **NOTE: this is different from the default!**
1. PyCharm should now be able to connect to the docker container and upload some helper files
1. Create default path mappings
 1. Select project directory and specify it is located at `/phovea`
 1. In the end the overview page should state: `<Project root> -> /phovea`
1. Gevent compatible Python debugging
 1. Go to *Build, Execution, Deployment* -> *Python Debugger*
 1. Check `Gevent compatible`
 1. Uncheck `PyQt compatible`
1. Apply changes -> Close settings dialog

![PyCharm Python debugger](/assets/images/doc_screenshots/pycharm_python-debugger.png )

## Create a phovea_server debug launch configuration

1. *Run* -> *Edit Configurations*
1. In the dialog *Plus sign* (top-left) -> Python
 * Name: `phovea_server`
 * Script: `\phovea\phovea_server\__main__.py`
 * Script parameters: `--env=dev api`
 * Python interpreter: Remote Python ([created above](#create-remote-python-interpreter))
 * Working directory: `\phovea`
 * Uncheck `Add content roots to PYTHONPATH` and `Add source roots to PYTHONPATH`
1. Apply and close the dialog

![PyCharm debug launch configuration](/assets/images/doc_screenshots/pycharm_debug-launch-config.png)

## Run the server using PyCharm

1. [Start Docker using the debug mixin](#prepare-environment)
1. Set a breakpoint next to the line number in a Python file
1. *Run* -> *Debug* -> *phovea_server*
1. Open the URL in the browser that triggers the breakpoint
1. Debug the Python in the PyCharm panel
