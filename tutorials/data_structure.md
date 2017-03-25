---
layout: documentation
title: Data Structures Intro
order: 3
---

Phovea supports simple loading and handling of various types data. It provides data structures for lazy loading and accessing data using promises. These data structures are implemented in the [core plugin](https://github.com/phovea/phovea_core).

Phovea currently supports the following data types: 

 * Matrices - a tabular structure of rows and columns where all columns and rows are of the same data type.
 * Tables - a tabular structure of rows and columns where the columns can be of different data types, e.g., different numerical ranges, mixed categorical, string, and numerical, etc.
 * Vectors - a 1-D data structure where all values have to be of the same type that can be used individually but that's also used in matrices and tables.
 * Stratifications - a data structure used for grouping elements.
 
**In addition to this high-level description of the dataset concepts, please refer to the [Phovea Demo Application](https://github.com/Caleydo/phovea_demos/) and to the [source code](https://github.com/phovea/phovea_core) for information on how to use these.** 

## Loading Datasets

The way data is accessed in Phovea can vary. For example, data could be loaded from a .csv file or retrieved from an SQL database. In the end, it up to different plugins and concrete implementations of data structure interfaces how the data is accessed.

The function [`loadLocalData()` in the demo project](https://github.com/Caleydo/phovea_demos/blob/master/src/UsingTable.ts) shows how you can load a CSV file from a local directory. Note that the data has to be hosted in a top-level data directory (TODO: is that true?) and that you also have to provide a JSON file called `index.json` describing the dataset - more on that in the next section. 

An alternative way is to use the **parseRemoteMatrix** method specified in `phovea_d3/parser`. It takes the dataset file path as argument and returns a promise for an associated data structure object.

An alternative to loading a local dataset is to run a server that loads the data for you, and then retrieve the data. To do that, you have to run a server via docker using the [workspace approach](getting_started/#server-client-app) and store the csv file in the top-level `data` folder and again provide a JSON file called `index.json` in the same directory. If you do that, the server will automatically load the dataset, and the dataset will be accessible via various functions (more on that later).

### Dataset Parsing

Tabular datasets in a form of .csv files can be loaded by providing the dataset file itself and a definition of the dataset. The definition is provided in an `index.json` file and could look like this:

```json
[
  {
    "id": "anscombe_II",
    "name": "Anscombe II",
    "path": "anscombe_II.csv",
    "type": "matrix",
    "size": [12, 2],
    "rowtype": "row",
    "coltype": "dimension",
    "separator": ";",
    "quotechar": "\"",
    "value": {
      "type": "real",
      "range": [0, 12]
    }
  }
]

```

See also the [examples here](https://github.com/Caleydo/phovea_demos/blob/master/data/index.json)

The following properties are common to all supported dataset types:

* `id` A string specifying a dataset id.
* `name` A string specifying the name of the dataset.
* `path` The path to the data file.
* `type` The dataset type. Currently `matrix`, `table`, `vector`, and `stratification` are supported.
* `size` The size of the dataset. In case of a matrix, number of rows and columns.
* `separator`: String that is used as separator in the data file. Default: ",".
* `quotechar`: String that is used to indicate how to escape cell content, so that the separator can be used within a cell. For example, with separator `,`, this wouldn't work: `red, green`. Instead, we use `"red, green"`. Defaults to `|`.

An `index.json` file may contain definitions for multiple datasets. 

### Dataset Access

There are two basic ways to access the data files from the client code. The first way is the **get** method specified in `phovea_core/data`. It takes the dataset id specified in the dataset defininition as parameter and returns a promise for an associated data structure object (Matrix, Table, Vector, Stratification). Note that this method involves code from the server side of Phovea and thus will not work with the lightweight client library version. 

Another way to access datasets is to use the **list** method from `phovea_core/data`. This method looks for dataset definitions in all Phovea plugins. All found datasets are accessible via a promise to an array of datastructure objects. This method will not work in the client library version.

See the various ways to access dataset in the method [`loadDataFromServer()` in the demo project](https://github.com/Caleydo/phovea_demos/blob/master/src/UsingTable.ts).

## Data Structures

All data structures are part of the [core plugin](https://github.com/phovea/phovea_core). Currently, the following data structures are supported: *Matrix*, *Table*, *Vector*, *Stratification*.

### Matrix

We consider a matrix as a two-dimensional data structure with rows and columns. Both rows and columns specify ids for different entities, for example, the rows are patients and the columns are genes. As a result, all values in the matrix have the same meaning, such as "gene expression".

|         | Gene 1           | Gene 2  |  Gene 3 |
| ------------- |:-------------:| :-----:|:-----:|
| **Patient 1**     | 5.4 | 4.1 | 8.3 |
| **Patient 2**     | 0.8      |   2.0 |0.0 |
| **Patient 3** | 3.2   |    7.7 |10.0 |  

#### Data File Definition

The definition for a matrix datafile could look like this:

```json
[
  {
    "name": "Expression Data",
    "id": "expression_data",
    "path": "expression.csv",
    "type": "matrix",
    "size": [3, 3],
    "rowtype": "patient",
    "coltype": "gene",
    "separator": ";",
    "value": {
      "type": "real",
      "range": [0, 10]
    }
  }
]

```

Consider the following properties for a matrix definition:

* `rowtype` Id type for the rows.
* `coltype` Id type for the columns.
* `value` Description of the values in the matrix. The `type` property specifies the data type. Valid values `real`, `int`, `categorical`, and `string`. `range` specifies minimum and maximum values for numerical data types.

#### Usage

Phovea matrices implement the [IMatrix](http://data.caleydo.org/builds/lib/docs/interfaces/_caleydo_core_matrix_.imatrix.html) interface, which can be extended to provide custom matrix implementations. Important attributes include:

* `rowtype` IDType (todo) for the matrix rows.
* `coltype` IDType (todo) for the matrix columns.
* `valuetype` Description of the values in the matrix, i.e., type, range etc. as specified in the definition file.
* `t` Transposed version of the matrix.

Methods to access the data make use of range (todo) parameters to specify a subset on the data. If no parameter is provided, the maximum range, i.e., all data is considered. Most methods also return promises (todo) instead of directly returning resulting values. Here are some important methods of a matrix:

* `data(range)` Returns a promise to the data of the matrix. If a 1D range is provided instead of a 2D range, only the rows are subsetted and all columns are returned.
* `rows(range)` Returns a promise to the row names of the matrix.
* `rowIds(range)` Returns a promise to the row ids specified as range.
* `cols(range)` Returns a promise to the column names of the matrix.
* `colIds(range)` Returns a promise to the column ids specified as range.
* `view(range)` Returns a view on the matrix, which can be used like a matrix object, but only consists of the subset specified by the range parameter.

Example:

```javascript

parser.parseRemoteMatrix('./data/anscombe_2.csv').then(function (matrix) {

  var v = matrix.view(ranges.parse("0:5", "0:-1"));

  Promise.all([v.data(), v.rows()]).then(function (promise) {

    console.log("Matrix data: " + promise[0].toString());
    console.log("Row names: " + promise[1].toString());
  }
});

```



### Table

Like the matrix, a table is a two dimensional data structure with rows and columns. In contrast to the matrix, only the rows specify entities using ids, whereas the columns represent different attributes of an entity. For example, if rows represent patients, the columns could represent attributes like gender or age.

| Patient        | Gender        | Age  |  Value |
| :-------------: |:-------------:| :-----:|:-----:|
| Patient 1     | male | 20 | 0.3 |
| Patient 2     | female      |   55 |0.1 |
| Patient 3 | male   |    32 |1.0 |

#### Data File Definition

This is how the definition of a table file could look like:

```json
  {
    "name": "Test Heterogeneous 10x4",
    "path": "test_h10x4.csv",
    "id": "test_h10x4",
    "size": [10, 4],
    "type": "table",
    "idtype": "patient",
    "quotechar": "\"",
    "columns": [
    {
      "name": "Gender",
      "value": {
        "type": "categorical",
        "categories": [
          {
            "name": "male",
            "color": "blue"
          },
          {
            "name": "female",
            "color": "red"
          }
        ]
      }
    },
    {
      "name": "Age",
      "value": {
        "type": "int",
        "range": [0, 100]
      }
    },
    {
      "name": "Value",
      "value": {
        "type": "real",
        "range": [0, 1]
      }
    }
  ]
}
```


Have a look at the following properties of a table definition:

* `idtype` Specifies the id type of the rows.
* `columns` As all columns refer to different attributes, their meaning has to be specified. The `name` property refers to the column name. The `value` property is to be interpreted the same way as for matrices, but here one it only valid for the corresponding column instead of the whole dataset.

#### Usage

Tables in Phovea are implementations of the [ITable](http://caleydo.gehlenborg.com/builds/lib/docs/interfaces/_caleydo_core_table_.itable.html) interface. An attribute worth mentioning is the `rowtype`, which specifies the id type of the rows. Many methods of the table such as `data(range)`´, `rows(range)`, or `view(range)` work similar to the matrix methods, using ranges as parameters and promises for return values. The main difference is how columns are handled: `cols(range)` returns a promise to an array of [vector](#vector) objects, one vector for each column. Here is a usage example:

```javascript

data.get('test_h10x4').then(function (table) {

  Promise.all([table.data(), table.cols()]).then(function (promise) {

    console.log("All table data: " + promise[0].toString());
    var firstColumnVector = promise[1][0];

    firstColumnVector.data().then(function(vectorData){
      console.log("Data of first Column: " + promise[0].toString());
    });
  });
});

```

### Vector

A vector is a data structure that associates an id with a single attribute value. Thus, a vector can be thought of one column of a [table](#table), or as a table with a single attribute column.


| ID        | Value        |
| :-------------: |:-------------:|
| Patient 1     | 0.3 |
| Patient 2     | 1.0      |
| Patient 3 | 0.8   |
| Patient 4 | 0.4   |
| Patient 5 | 0.6   |

#### Data File Definition

This is how the definition of a vector looks like when it is loaded from a file:

```json
{
  "name": "Patient Vector",
  "path": "vector.csv",
  "size": 10,
  "type": "vector",
  "idtype": "patient",
  "value": {
    "type": "real",
    "range": [0, 1]
  }
}
```

Here, the `idtype` refers to the id type of the columns, whereas the `value` describes the values of the vector, just like in the matrix definition or the definition of table columns.

#### Usage

Vectors implement the [IVector](http://data.caleydo.org/builds/lib/docs/interfaces/_caleydo_core_vector_.ivector.html) interface.

### Stratification

| ID        | Group        |
| :-------------: |:-------------:|
| Patient 1     | one |
| Patient 2     | two      |
| Patient 3 | one   |
| Patient 4 | two   |
| Patient 5 | two   |

```json
{
  "name": "D2 Column KMeans 3",
  "origin": "demo_app/D2",
  "path": "d2t.3.csv",
  "separator": "\t",
  "type": "stratification",
  "idtype": "patient",
  "size": 5,

  "ngroups": 3,
  "groups": [
    {
      "name": "one",
      "size": 2
    },
    {
      "name": "two",
      "size": 3
    }
  ],

  "ws": "random"
}
```

#### Usage

Stratifications implement the [IStratification](http://data.caleydo.org/builds/lib/docs/interfaces/_caleydo_core_stratification_.istratification.html) interface.

## OLD CONTENT

Range
-----

source files: range.ts, range.py

caledyo_core
------------
 * main.ts ... mainly utilities, e.g. for accessing get parameters
 * event.ts ... event mechanism using global events class inheritance
 * math.ts ... simple stats helper
 * ajax.ts ... wrapper around different ajax provider (JQuery, D3) using a promisified interface
 * geom.ts, 2d.ts ... 2d plane operations and utilities, e.g. bounding boxes
 * range.ts ... range implementation
 * datatype.ts ... datatype definition file
 * (vector|matrix|table|stratification)[_impl].ts, definition and implementation of standard data types
 * data.ts ... data query/filter and convert manager
 * idtype.ts ... id type and selection manager
 * plugin.ts ... unified access to plugins defined in the registry
 * vis.ts ... custom wrapper around plugin.tx for vis plugins
 * multiform.ts ... implementation of the multiform approach, e.g a proxy around different vis types

Datatypes
---------
Datatypes are just a specific plugin type implementing a minimal interface `datatypes.IDataType`

basic principles:
 * *metadata*

   the description contains the most important meta data (types, dimensions, value types, ranges,...)

 * *subsetting/views*

   subsetting a matrix/vector results in another matrix/vector. e.g. a taking just the first half of vector is just another vector, the visualization doesn't have to even know that. A subset is just a special view on the raw data no data is copied unless needed

 * *ids and annotations*

   every annotation (e.g. gene symbol EGFR) is assigned a unique integer id on the sever side (e.g. 5). ids are used for selection since they are more efficient to handle than strings. Actually the ids of a dataset are accessible as a multi-dimensional `range`.

 * *selection work in the dataset domain*
   a visualization just selects data within its dataset. e.g. the first row/column is selected nothing more. Similarly the (global) selections are converted to the dataset domain. e.g. the vis will be notified that rows 1 to 3 are selected. Internally the row indices are converted to their corresponding unique id numbers and these are selected within the idtype.

server side principles

 * datastores

   datastores are plugins that support different types of storage. current ones https://github.com/phovea/?utf8=%E2%9C%93&query=caleydo_data + a default one for [CSV files](https://github.com/phovea/phovea_server/blob/master/dataset_csv.py). A datastore is a unified way to list, upload, delete, and modify a datsets.

 * numpy

   matrixes and vectors are accessible as numpy arrays. ranges support conversions to numpy selectors

 * TODO: filter, selection, query



vis.ts
------

visualization `vis.IVisIntance`, factory pattern: `create(data: datatypes.IDataType, parent: DOMElement, options)`
