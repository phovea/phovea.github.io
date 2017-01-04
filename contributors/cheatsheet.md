---
layout: documentation
title:  Cheatsheet
order: 2
---

Prerequisite: Install Phovea generator
--------------------------------------

```
sudo npm install -g yo github:phovea/generator-phovea
```

Initialize a new Phovea plugin
------------------------------

```
mkdir name_of_new_plugin
cd name_of_new_plugin
yo phovea
```

Depend on another plugin or library
-----------------------------------

```
cd name_of_new_plugin
yo phovea:add-dependency
```


Define a new extension point
----------------------------

```
cd name_of_new_plugin
yo phovea:add-extension
```

Launch a Phovea application
----------------------------

```
cd name_of_new_plugin
npm install
npm run start
```

Update project template
-----------------------

to get latest project updates and bugfixes

```
npm install -g github:phovea/generator-phovea
cd name_of_plugin
yo phovea:update
```

Use a workspace
--------------------

If you are developing multiple plugins at the same time a "workspace"
using [Docker](https://www.docker.com/) and 
[Docker Compose](https://www.docker.com/products/docker-compose)
can make the process easier. Creating a workspace this way also generates a PyCharm project.
The basic idea is to use the parent directory of the plugins, so that they can
share a common npm installation and docker setup.

Make sure you have NPM, Yeoman, [Docker](https://docs.docker.com/engine/installation/)
and [Docker Compose](https://docs.docker.com/compose/install/) installed.

```
mkdir workspace
cd workspace

git clone https://github.com/phovea/phovea_server.git
# AND/OR clone any other repos that you will need. 
yo phovea:resolve

yo phovea:workspace
npm install
docker-compose up
```

To add other Phovea plugins, halt Docker by hitting ctrl-C, and then:

```
# For example, if you want to support interactions with a SQL database:
git clone git@github.com:phovea/phovea_data_sql.git
yo phovea:resolve

yo phovea:workspace
npm install
docker-compose build api
docker-compose up
```

### Launch a Phovea application

```
npm run start:name_of_application
```

### Clone an existing Phovea plugin

```
cd workspace
yo phovea:clone
```

### For Each

e.g., pull all git repos

```
cd workspace
./forEach git pull
```

Migration
---------

The wizard guides you through the different steps for migrating a Caleydo plugin to Phovea.

```
cd some_directory
yo phovea:migrate-wizard
```
