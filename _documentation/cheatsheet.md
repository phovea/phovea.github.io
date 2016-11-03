---
layout: documentation
title:  Cheatsheet
permalink: /cheatsheet/
---

Prerequisite: Install Phovea generator
--------------------------------------

```
sudo npm install -g yo gitbub:phovea/generator-phovea
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

Ueber Related: Launch a Phovea application/service
--------------------------------------------------

```
npm run start:name_of_application
```


Ueber Related: Clone an existing Phovea plugin
----------------------------------------------

```
cd ueber_directory
yo phovea:clone
```

Ueber Related: Resolve dependencies of plugin
---------------------------------------------

i.e., clone the plugins dependencies into the ueber context

```
cd ueber_directory
yo phovea:resolve
```
