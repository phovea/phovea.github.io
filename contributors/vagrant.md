---
layout: documentation
title:  Setting up Vagrant TODO
order: 1000
---

**WARNING**: This information may be out of date.
If you know what is currently correct, please make a PR.

Note: Setting up a dev environment requires a working installation of Git!

- *Windows Only*: Install [Git](http://git-scm.com/download/win)

- Install [Vagrant](http://www.vagrantup.com/) and [VirtualBox](https://www.virtualbox.org/)
    - OS X:  `brew cask install vagrant virtualbox`

  Vagrant is used for creating a controlled environment using a virtual machine provided by VirtualBox.

- Clone this repository

```bash
 git clone https://github.com/phovea/phovea_web_container.git
```

- Launch a (bash) shell
  
  *Windows Only*: Ensure that you start the `Git Bash` with administrative rights

- Switch to the new directory

```bash
 cd phovea_web_container
```

- Let Vagrant create the environment for you

```bash
 # start vagrant
 vagrant up
```

- Connect to VM

```bash
 # connect to vm
 vagrant ssh
```

- Navigate to phovea directory

```bash
 cd /vagrant
```

The `/vagrant` folder is shared with your cloned repository. So, all changes are reflected in your local filesystem.

- Exit and stop the virtual machine

```bash
 exit
 vagrant halt
```

