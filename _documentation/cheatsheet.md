---
layout: documentation
title:  Cheatsheet
permalink: /cheatsheet/
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

Use an ueber context
--------------------

An ueber context helps you developing multiple plugins at the same time and working with server side plugins using [Vagrant](https://www.vagrantup.com). 
The basic idea is to use the parent directory of the plugins, share a common npm installation and vagrant setup

```
mkdir ueber_directory
cd ueber_directory
# initialize/clone/resolve plugins
yo phovea:ueber
npm install
vagrant up
```

### Launch a Phovea application/service

```
npm run start:name_of_application
```

### Clone an existing Phovea plugin

```
cd ueber_directory
yo phovea:clone
```

### Resolve dependencies of plugin

i.e., clone the plugins dependencies into the ueber context

```
cd ueber_directory
yo phovea:resolve
```

### For Each

e.g., pull all git repos

```
cd ueber_directory
./forEach git pull
```

Migration
---------

The wizard guides you through the different steps for migrating a Caleydo plugin to Phovea

```
cd some_directory
yo phovea:migrate-wizard
```
