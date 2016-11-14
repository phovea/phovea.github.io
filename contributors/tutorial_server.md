---
layout: documentation
title:  Server Tutorial TODO
order: 1000
---

```bash
git clone https://github.com/phovea/phovea_web_container.git
cd phovea_web_container
vagrant up
# Wait and get some coffee. This will take a while.
vagrant ssh
```

Then, from within the VM:

```bash
# 1. Clone the repositories and their dependencies
./manage.sh clone phovea_sample_app
./manage.sh clone phovea_server

# 2. Resolve dependencies of plugins
./manage.sh resolve

# 3. Start phovea web
./manage.sh server
```

Go to [http://localhost:9000](http://localhost:9000).
