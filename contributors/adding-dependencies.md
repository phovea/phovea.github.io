---
layout: documentation
title: Adding dependencies
order: 2
---

When you create a plugin or application it is common to use additional Phovea modules or external libraries to avoid *re-inventing the wheel* again. There are slightly different ways how to add dependencies for the [client-side](#client-dependencies) and the [server-side](#server-dependencies). 


<a id="client-dependencies"></a>

## Adding client-side dependencies

A client-side dependency can be another Phovea modules or JavaScript library that is available via the [npm registry](https://www.npmjs.com/).

### Phovea modules and additional libraries

1. Open the command line and navigate to your application/plugin directory
2. Run `yo phovea:add-dependency`
3. Select the *additional Phovea modules* that should be included
4. Confirm your selection with Enter and move the next step
5. Select *additional libraries* (e.g., jQuery, d3) that are known by the Phovea framwork
6. Confirm your selection with Enter
7. Override files if you will be asked to
8. Commit the changes to your application / plugin repository (optional, but recommended)
9. [Update your setup](#update-setup)


### Add other external libraries

Follow theses steps only, if you cannot find the library in the known libraries by the Phovea framework (using the Phovea generator in the step above).

1. Research if the library and the *@types* (TypeScript typings) are available on [npmjs](https://www.npmjs.com/)
2. If yes, continue. If not, ask the library author to publish the library on npmjs.
3. Open the *package.json* of the application/plugin repository
4. Add the library and typings in the corresponding versions to the [dependencies section](https://docs.npmjs.com/files/package.json#dependencies), as follows:

  ```json
  "dependencies": {
    "@types/d3": "3.5.36",
    "d3": "3.5.17"
  },
  ```
   
5. [Update your setup](#update-setup)

-----

<a id="server-dependencies"></a>

## Adding server-side dependencies


### Phovea server plugins

1. 


### pip dependencies


-----

<a id="update-setup"></a>

## Update setup / apply changes

### Workspace setup

1. Navigate to the workspace directory
2. Run `yo phovea:workspace` to merge the files from all cloned repos
3. Override files if you will be asked to
4. Run `yo phovea:resolve` and `npm install`


### Stand-alone setup

1. Run `yo phovea:resolve` and `npm install`


