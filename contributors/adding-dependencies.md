---
layout: documentation
title: Adding dependencies
order: 2
---

When you create a plugin or application it is common to use additional Phovea modules or external libraries to avoid *re-inventing the wheel* again. There are slightly different ways how to add dependencies for the [client-side](#client-dependencies) and the [server-side](#server-dependencies). 


<a id="client-dependencies"></a>

## Adding client-side dependencies

A client-side dependency can be another Phovea module or a JavaScript library available on the [npm registry](https://www.npmjs.com/).

### Phovea modules and additional libraries

1. Open the command line and navigate to the plugin directory
2. Run `yo phovea:add-dependency`
3. Select the *additional (Phovea) modules* that should be included
4. Confirm your selection with Enter and move the next step
5. Select *additional libraries* (e.g., jQuery, D3) that are known by the Phovea framwork
6. Confirm your selection with Enter
7. Override files if you will be asked to
8. (Optional, but recommended: Commit the changes)
9. [Update your setup](#update-setup)

<a id="add-external-libs"></a>

### Add other external libraries

Follow theses steps only, if you cannot find the library in the known libraries by the Phovea framework (using the Phovea generator in the step above).

1. Research if the library and the *@types* (TypeScript typings) are available on [npmjs](https://www.npmjs.com/)
2. If yes, continue. If not, ask the library author to publish the library on npmjs.
3. Open the *package.json* of the application/plugin repository
4. Add the library and typings in the corresponding versions to the [dependencies section](https://docs.npmjs.com/files/package.json#dependencies). For example:

  ```json
  "dependencies": {
    "@types/d3": "3.5.36",
    "d3": "3.5.17"
  },
  ```
   
5. (Optional, but recommended: Commit the changes)
6. [Update your setup](#update-setup)


### Version conflicts with D3 (v3 vs. v4)

Most of the Phovea repositories (e.g., [phovea_vis](https://github.com/phovea/phovea_vis/) and [phovea_clue](https://github.com/phovea/phovea_clue)) use D3 v3. If you want to use code that uses D3 v4 add the specific D3 module (e.g., [d3-selection](https://github.com/d3/d3-selection)) and the @type dependency (as [described above](#add-external-libs)) to the *package.json*.

```json
"dependencies": {
   "@types/d3-scale": "^1.0.4",
   "d3-scale": "^1.0.3"
},
```

Note, that you must import the specific D3 v4 modules to your TypeScript file as follows to avoid conflicts with the D3 v3 library:

```js
import {scaleLinear} from 'd3-scale';
```

-----

<a id="server-dependencies"></a>

## Adding server-side dependencies

A server-side dependency can be another Phovea module or pip package available on [PyPI Python Package Index](https://pypi.python.org/pypi).

### Phovea server modules and additional libraries

1. Open the command line and navigate to the directory of the server library 
2. Run `yo phovea:add-dependency`
3. Select the *additional (Phovea) modules* that should be included
4. Confirm your selection with Enter and move the next step
5. Select *additional libraries* (e.g., MongoDB, Redis) that are known by the Phovea framwork
6. Confirm your selection with Enter
7. Override files if you will be asked to
8. (Optional, but recommended: Commit the changes)
9. [Update your setup](#update-setup)


### pip dependencies

Follow theses steps only, if you cannot find a suitable Phovea plugin (using the Phovea generator in the step above).

1. Navigate to the server library directory
2. Open the *requirements.txt*
3. Add the pip dependencies and version (one per line) as follows:
   
  ```
  ujson==1.33
  enum==0.4.6
  ```
  
4. (Optional, but recommended: Commit the changes)
5. [Update your setup](#update-setup)

**NOTE:** A bug ([addressed here](https://github.com/phovea/generator-phovea/issues/81)) in the Phovea generator is overriding changes made to the *requirements.txt* when running `yo phovea:update`. You have to restore the modifications manually until the bug is fixed.

-----

<a id="update-setup"></a>

## Update setup / apply changes

### Workspace setup

1. Navigate to the workspace directory
2. Run `yo phovea:workspace` to merge the dependencies of all cloned repos in the workspace directory
3. Override files if you will be asked to
4. Run `docker-compose build` for server-side changes
5. Run `npm install` for client-side changes


### Stand-alone setup

1. Run `npm install` to install new npm dependencies


