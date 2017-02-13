---
layout: documentation
title:  Getting Started
order: 2
---

## Getting Started With Phovea Applications

*The examples below are all for client-side only applications. While I don't anticipate the server side components add too much complexity, keep that in mind when venturing outside of the scope of these examples.*

There are two ways of getting a Phovea Application up and running:

 1. Clone an existing app and run it locally on your machine.
 2. Create a new one from scratch

We will start with cloning an existing application as this will help you understand where all the pieces go and where/how you should write you code when writing your own application.

## Cloning and building an existing Phovea Application

*examples below use the [genealogyVIS](https://github.com/Caleydo/genealogyVIS/) application but can be use for any existing Phovea application*

There are two ways to set up an existing Phovea App

 1. Create a new directory (this will avoid dependency conflicts with other tools), clone  one ore more app repositories, create a workspace, install and run the application from the workspace 
 2. Clone an app repository, install, build (optional), and run according to the instruction in the repo readme 

*The first approach (creating a workspace) is particularly useful when developing multiple plugins, as all the projects within the workspace will share a common npm installation and Docker setup.* 

### Workspace Approach 
*This approach creates a ''workspace'' which uses [Docker](https://www.docker.com/) and [Docker Compose](https://docs.docker.com/compose/). The workspace setup also creates a [PyCharm](https://www.jetbrains.com/pycharm/) project.* 

 1. `mkdir workspace`
 2. `cd workspace`
 3. `git clone https://github.com/Caleydo/genealogyVIS.git` (and/or) clone any other repos that you will need). 
 4. Create workspace `yo phovea:workspace`
 5. Install dependencies `npm install`
 6. Run application `npm run start:genealogyVIS`
 7. Open browser and navigate to http://localhost:8080/

Your directory structure (for one or more projects) will look like this:

```
workspace --> has the helper/docker files but is not attached to any repo
\- repo1 --> cloned from github
\- repo2 --> cloned from github
```

If you have several repos in your workspace, you can run: 

1. forEach git pull or 
2. forEach git push  

to update all the repos in your workspace. 

### Direct, non-workspace, Approach 


#### Installing 

```bash
$ git clone https://github.com/Caleydo/genealogyVIS.git
$ cd genealogy_vis
$ npm install
```

#### Building (optional)

```bash
$ npm run build
```

The build process is optional because it is not required in order to run the application (e.g. during testing/development). 


The build process itself includes:
* running all unit tests -> karma
* checking code quality -> tslint
* building the application in production mode (i.e, creates compiled, minified and chunked javascript files that can be loaded using [Webpack](https://webpack.js.org)) == ready to be deployed


#### Launching
`npm run start`

Unlike `npm run build`, `npm run start` will just build the application *in memory* without any tests or code quality checks


#### Notes on using javascript files in a Phovea App

Phovea ignores \*.js files on commit. since TypeScript is a superset of javascript it's safe to rename \*.js to \*.ts files. The only thing you have to take care about is the import and export, since each file is a separate module/file.

The need for import and export is because with TypeScript (and ES6 in general) you cannot use the global scope and every file has his own scope. You need to declare what variables and functions should be exported/imported. Follow the links to learn more about [export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) and [import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import).

Now that you have successfully (hopefully) cloned and examined an existing application, you are ready to build your own application!

## Creating a new Phovea Application from Scratch

 1. Install Phovea Generator `sudo npm install -g yo github:phovea/generator-phovea`
 2. `mkdir name_of_new_app`
 3. `cd name_of_new_app`
 4. Run `yo phovea` and follow the prompts. 

### Installing

`npm install`

### Building (optional)
`npm run build`

The build step is optional since it is not required for testing/development. 

The build process itself includes:
 * running all unit tests -> karma
 * checking code quality -> tslint
 * building the application in production mode (i.e, generates compiled, minified and chunked javascript files that can be loaded using webpack https://webpack.js.org) = ready to be deployed


### Launching

`npm start`

## Creating a new Phovea Server-Client Application from Scratch

 1. Install [Docker](https://www.docker.com/).
 2. Install Phovea Generator `sudo npm install -g yo github:phovea/generator-phovea`
 2. `mkdir app_folder`
 3. `cd app_folder`
 
Create the client side as follows:

4. `mkdir app`
5. `cd app`
6. Run` yo phovea:init-app` and follow the prompts. 
7. `cd ..`

Create the server side as follows:

8. `mkdir app_server`
9. `cd app_server`
10. Run `yo phovea:init-slib` and follow the prompts
11. `cd ..`

Clone and resolve the `phovea_server` as follows:

12. Run `yo phovea:clone phovea_server` and follow the prompts.
13. `yo phovea:workspace`

### Installing

Keep Docker running.

1. `npm install`
2. `docker-compose build`

### Launching

Keep Docker running.

1. `docker-compose up -d`
2. `npm run start:app`

Now in the browser check adress `http://localhost:8080/` for accessing the app or `http://localhost:8080/api/hello_world/` for accessing the api.






