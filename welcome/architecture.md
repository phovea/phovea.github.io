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
2. Provides infrastructure for managing data, synchronizing views (event handling), and tracking user actions (provenance tracking).
3. Can be used as client-only Javascript library or a coupled client-server system.
4. Client-side code is written in TypeScript (static typing, class and module support, ES6 support, etc...).
5. Server-side code is written in Python (Flask framework).


## Phovea Repository Types

Phovea's modular and flexible archticture requires a sub-division of functionality. Therefore we distribute the functionality in different repositories and distinguish between three different types:

### Plugin

* Is a repository that can be linked during build time
* Contains a group of related functionalities and files
* The repository name can be choosen freely
* Examples: [phovea_importer](https://github.com/phovea/phovea_importer/)
 
### Application
* Is a special type of plugin 
* Contains an *index.html* and an own user interface (*app.ts*)
* The repository name can be choosen freely
* Examples: [LineUp](https://github.com/Caleydo/lineup), or [TaCo](https://github.com/Caleydo/taco)

### Product
* Is a deployable configuration consisting of one or multiple plugins
* The repository name must have the suffix *\_product*
* Phovea can clone all dependant repositories, download data packages, install dependencies, and build Docker images
* Tip: Use `yo phovea:setup-workspace <product>` to get started right away
* Examples: [lineup_product](https://github.com/Caleydo/lineup_product), or [taco_product](https://github.com/Caleydo/taco_product)


## Core Phovea repositories (and what they provide)

The list shows the main repositories that are widely used accross all applications:

1. [Phovea Core](https://github.com/phovea/phovea_core/) - data structures and events
2. [Phovea Vis](https://github.com/phovea/phovea_vis/) - basic visualizations
4. [Phovea UI](https://github.com/phovea/phovea_ui/) - user interface elements (fonts, headers,etc)
3. [Phovea Clue](https://github.com/phovea/phovea_clue/) - provenance graphs (UI for provenance graphs, uses graph data type)

Further repositories can be found in the [Phovea organization](https://github.com/phovea/).


## Phovea Client Components

Each repository contains a set of related files. However, *re-inventing the wheel* should be avoided and re-using existing functionalities from other repositories should be prefered. 

In Phovea we distinguishes between **TypeScript modules** and **extensions**, which are a special kind of TypeScript modules.
Both have in common that they are small reusable components. However, the main difference is that extensions tend to be used by multiple apps while TypeScript modules are typically only used by their own application.

### TypeScript Modules

* TypeScript modules provide the possibility to group related logic, encapsulate it, structure your code and prevent pollution of the global namespace.
* TypeScript modules can provide functionality that is only visible inside the module, and they can provide functionality that is visible from the outside using the export keyword.

Learn more about TypeScript modules in the [TypeScript documentation](https://www.typescriptlang.org/docs/handbook/modules.html).

### Extensions

* Extensions are are registered TypeScript modules, which means that they can be imported by other plugin.
* An extension is identified by a unique ID, extension type, and the TypeScript module that implements this extension.
* Exentions are optional, i.e. a plugin can have none or multiple extensions.

Example of the phovea_vis/vis.ts extension being registered in the *phovea.js* file:
 
```js
  registry.push('vis', 'table', function () {
    return System.import('./src/table');
  }, {
    name: 'Table',
    filter: '(matrix|table|vector)',
    sizeDependsOnDataDimension: true

  });
```
 
Currently there are two types of extensions:

1. `datatype` ->  all the data structures provided by phovea_core 
2. `vis` -> 'pre made' visualizations provided by phovea_vis.



## Phovea Data Structures

The current set of Phovea data structures provided in the [Phovea core](https://github.com/phovea/phovea_core) are: 

1. Matrix
2. Table
3. Vector
4. Stratification
5. Graph

All the data structure implement the IDataType Interface:
*source: https://github.com/phovea/phovea_core/blob/master/src/datatype.ts*
 
```ts
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

```ts
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











