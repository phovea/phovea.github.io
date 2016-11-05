---
layout: documentation
title:  Data Structures
order: 2
---


(This example is also available at
[jsfiddle](http://jsfiddle.net/gh/get/library/pure/phovea/phovea.github.io/tree/master/tutorials/demo_data_structures/jsfiddle).)

Ranges in Phovea can be used to subset your data and zoom in on regions of interest.

```javascript
{% include_relative demo_data_structures/demo_5.js %}
```
<iframe src="../frame.html?demo_data_structures/demo_5" height="1100"></iframe>

Note that visualizations based on the same backing data are linked: if you click on a
datapoint in one visualization, the corresponding datapoints in other visualizations
will also be highlighted

A number of primitive methods are supported on ranges so you can create
and manipulate them programmatically.

```javascript
{% include_relative demo_data_structures/demo_6.js %}
```
<iframe src="../frame.html?demo_data_structures/demo_6"></iframe>
