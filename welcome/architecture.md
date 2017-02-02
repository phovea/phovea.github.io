---
layout: documentation
title:  Architecture
---

In Phovea, all components are plugins. 
The client code is written in TypeScript (transpiled to JavaScript)
and uses HTML5 APIs, 
while the server is implemented in Python and uses the 
[Flask](http://flask.pocoo.org/) framework. 
Client and server communicate via REST and Websocket interfaces. 
