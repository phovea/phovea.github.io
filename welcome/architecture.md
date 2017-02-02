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



## What Phovea can do for you

1. Provides datatypes and basic visualization for genomic data.
2. Provides infrastructure for ​managing data, synchronizing views (event handling), and tracking user actions (provenance tracking).
3. Can be used as client-only Javascript library or a coupled client-server system.
4. Client-side code is written in Typescript (static typing, class and module support, ES6 support, etc...)
5. Server-side code is written in Python (Flask framework)


## Core Phovea Repositories and what they provide
*https://githb.com/phovea*

1. Phovea Core - data structures and events
2. Phovea Vis - basic visualizations
3. Phovea Clue - Provenance graphs (uses graph data type)
4. Phovea UI - User Interface elements (fonts, headers,etc)


## Phovea Components


### App or Plugin? 

An app is a special type of plugin that:
* has index.html
* has its own user interface (app.ts)

An app or plugin is developed in its own repo 

Example Apps: LineUp, TACO
Example Plugin: Importer

### Extension or Typescript Module?  

Both extension and typescript modules are smaller reusable components that can be used by one or more apps. 
They also don't have their own repos (single .ts file)

​​Extensions are special kinds of typescript modules​ and tend to be used by multiple apps while ts modules are typically only used by their own application.

### TypeScript Modules

* Modules provide the possibility to group related logic, encapsulate it, structure your code and prevent pollution of the global namespace.
* Modules can provide functionality that is only visible inside the module, and they can provide functionality that is visible from the outside using the export keyword.

### Extensions

Extensions are registered modules (phovea.js)
*to register = to allow for import by other applications.* 

Example of the phovea_vis/vis.ts extension being registered:
 
```
  registry.push('vis', 'table', function () {
    return System.import('./src/table');
  }, {
    name: 'Table',
    filter: '(matrix|table|vector)',
    sizeDependsOnDataDimension: true

  });
```
 
Currently there are two types of extensions:
    1. 'datatype' ->  all the data structures provided by phovea_core 
    2. 'vis' -> 'pre made' visualizations provided by phovea_vis.

### Phovea Data Structures
*https://github.com/phovea/phovea_core*

The current set of Phovea Data structures provided are: 

1. Matrix
2. Table
3. Vector
4. Stratification
5. Graph

All the data structure implement the IDataType Interface:
*source: https://github.com/phovea/phovea_core/blob/master/src/datatype.ts*
 
``` 
/**
 * basic data type interface
 */
export interface IDataType extends ISelectAble, IPersistable {
  /**
   * its description
   */
  desc: IDataDescription;
  /**
   * dimensions of this datatype
   * rows, cols, ....
   */
  dim: number[];


  idView(idRange?: Range) : Promise<IDataType>;
}
``` 


All data structures take as input data in the format described by the IDataDescription Interface: 
*source: https://github.com/phovea/phovea_core/blob/master/src/datatype.ts *

```
/**
    * basic description elements
    */
   export interface IDataDescription {
     /**
      * the unique id
      */
     id: string;
     /**
      * the type of the datatype, e.g. matrix, vector, stratification, ...
      */
     type: string;
   
     /**
      * the name of the dataset
      */
     name: string;
     /**
      * a fully qualified name, e.g. project_name/name
      */
     fqname: string;
   
     [extras: string]: any;
   }
   ```
   
   
### Phovea Visualizations
*https://github.com/phovea/phovea_vis*

 The current set of Phovea Visualizations are: 
 
 1. Axis
 2. Bar Plot
 3. Table
 4. HeatMap
 5. Histogram
 6. Mosaic
 7. Pie Chart
 8. Box Plot
 9. Force Directed graph
 
 * All phovea visualizations can be found in the phovea_vis repository.
 * They all take a phovea data structure (described above) as input. 


## Version Conflicts with D3 (v3 vs V4)

Since most of the phovea repositories that use d3 (e.g: phovea_vis and phovea_clue) were written with v3, any contributors wishing to include code that uses d3 v4 must import the specific modules to avoid conflicts. 

e.g: `import {scaleLinear} from 'd3-scale';
;`

Remember that the d3-modules you use (such as d-selection, etc.) must also be added to package.json both as a module and as an @type dependency. 

e.g: 

```
"dependencies": {
     "@types/d3-scale": "^1.0.4",
     "d3-scale": "^1.0.3",
     "phovea_core": "github:phovea/phovea_core#develop",
     "phovea_ui": "github:phovea/phovea_ui#develop"
   },
```

## Structure of a Phovea (client side) application. 

1. app.ts - starting point 
2. Views
3. Communications between views (Events)
4. Provenance tracking (Clue) 


### Views

1. Usually implemented as a typescript module
2. Creates its own html elements
3. Builds the view (e.g creates svg elements)
4. Binds change functions (i.e creates and listens for events) 


### Events
*https://github.com/phovea/phovea_core/blob/master/src/event.ts*

 `import * as events from 'phovea_core/src/event'`
 
 You can either fire events when the user does something on a view
 
  `events.fire ('clicked on important button' , myData);`
  
 or listen for events when the user interacts with another view that the current view should respond to.
 `events.on ('clicked on button in another view' , update())`











