# d3-line-chunked

[![npm version](https://badge.fury.io/js/d3-line-chunked.svg)](https://badge.fury.io/js/d3-line-chunked)

A d3 plugin that renders a line with potential gaps in the data by styling the gaps differently
from the defined areas. Single points are rendered as circles. Transitions are supported.

Demo: http://peterbeshai.com/vis/d3-line-chunked/

![d3-line-chunked demo](https://cloud.githubusercontent.com/assets/793847/18028989/5aa2ee0c-6c59-11e6-88ef-143a79715cc6.gif)

## Example Usage

```js
var lineChunked = d3.lineChunked()
  .x(function (d) { return x(d.x); })
  .y(function (d) { return y(d.y); })
  .curve(d3.curveLinear)
  .defined(function (d) { return d.y != null; })
  .lineStyles({
    stroke: '#0bb',
  });

d3.select('svg')
  .append('g')
    .datum(lineData)
    .transition()
    .duration(1000)
    .call(lineChunked);
```

## Development

Get rollup watching for changes and rebuilding

```bash
npm run watch
```

Run a web server in the example directory

```bash
cd example
php -S localhost:8000
```

Go to http://localhost:8000


## Installing

If you use NPM, `npm install d3-line-chunked`. Otherwise, download the [latest release](https://github.com/pbeshai/d3-line-chunked/releases/latest).

## API Reference

<a href="#lineChunked" name="lineChunked">#</a> d3.**lineChunked**()

Constructs a new generator for chunked lines with the default settings.


<a href="#_lineChunked" name="_lineChunked">#</a> *lineChunked*(*context*)

Render the chunked line to the given *context*, which may be either a [d3 selection](https://github.com/d3/d3-selection)
of SVG containers (either SVG or G elements) or a corresponding [d3 transition](https://github.com/d3/d3-transition).


<a href="#lineChunked_x" name="lineChunked_x">#</a> *lineChunked*.**x**([*x*])

Define an accessor for getting the `x` value for a data point. See [d3 line.x](https://github.com/d3/d3-shape#line_x) for details.


<a href="#lineChunked_y" name="lineChunked_y">#</a> *lineChunked*.**y**([*y*])

Define an accessor for getting the `y` value for a data point. See [d3 line.y](https://github.com/d3/d3-shape#line_y) for details.


<a href="#lineChunked_curve" name="lineChunked_curve">#</a> *lineChunked*.**curve**([*curve*])

Get or set the [d3 curve factory](https://github.com/d3/d3-shape#curves) for the line. See [d3 line.curve](https://github.com/d3/d3-shape#line_curve) for details.
Define an accessor for getting the `curve` value for a data point. See [d3 line.curve](https://github.com/d3/d3-shape#line_curve) for details.


<a href="#lineChunked_defined" name="lineChunked_defined">#</a> *lineChunked*.**defined**([*defined*])

Get or set *defined*, a function that given a data point (`d`) returns `true` if the data is defined for that point and `false` otherwise. This function is important for determining where gaps are in the data when your data includes points without data in them.

For example, if your data contains attributes `x` and `y`, but no `y` when there is no data available, you might set *defined* as follows:

```js
// sample data
var data = [{ x: 1, y: 10 }, { x: 2 }, { x: 3 }, { x: 4, y: 15 }, { x: 5, y: 12 }];

// returns true if d has a y value set
function defined(d) {
  return d.y != null;
}
```

It is only necessary to define this if your dataset includes entries for points without data.

The default returns `true` for all points.



<a href="#lineChunked_isNext" name="lineChunked_isNext">#</a> *lineChunked*.**isNext**([*isNext*])

Get or set *isNext*, a function to determine if a data point follows the previous. This function enables detecting gaps in the data when there is an unexpected jump.

For example, if your data contains attributes `x` and `y`, and does not include points with missing data, you might set **isNext** as follows:


```js
// sample data
var data = [{ x: 1, y: 10 }, { x: 4, y: 15 }, { x: 5, y: 12 }];

// returns true if current datum is 1 `x` ahead of previous datum
function isNext(previousDatum, currentDatum) {
  var expectedDelta = 1;
  return (currentDatum.x - previousDatum.x) === expectedDelta;
}
```

It is only necessary to define this if your data doesn't explicitly include gaps in it.

The default returns `true` for all points.



<a href="#lineChunked_transitionInitial" name="lineChunked_transitionInitial">#</a> *lineChunked*.**transitionInitial**([*transitionInitial*])

Get or set *transitionInitial*, a boolean flag that indicates whether to perform a transition on initial render or not. If true and the *context* that *lineChunked* is called in is a transition, then the line will animate its y value on initial render. If false, the line will appear rendered immediately with no animation on initial render. This does not affect any subsequent renders and their respective transitions.



<a href="#lineChunked_lineStyles" name="lineChunked_lineStyles">#</a> *lineChunked*.**lineStyles**([*lineStyles*])

Get or set *lineStyles*, an object mapping style keys to style values to be applied to both defined and undefined lines. Uses syntax similar to [d3-selection-multi](https://github.com/d3/d3-selection-multi#selection_styles).



<a href="#lineChunked_lineAttrs" name="lineChunked_lineAttrs">#</a> *lineChunked*.**lineAttrs**([*lineAttrs*])

Get or set *lineAttrs*, an object mapping attribute keys to attribute values to be applied to both defined and undefined lines. The passed in *lineAttrs* are merged with the defaults. Uses syntax similar to [d3-selection-multi](https://github.com/d3/d3-selection-multi#selection_attrs).

The default attrs are:

```js
{
  fill: 'none',
  stroke: '#222',
  'stroke-width': 1.5,
  'stroke-opacity': 1,
}
```



<a href="#lineChunked_gapStyles" name="lineChunked_gapStyles">#</a> *lineChunked*.**gapStyles**([*gapStyles*])

Get or set *gapStyles*, an object mapping style keys to style values to be applied only to undefined lines. It overrides values provided in *lineStyles*. Uses syntax similar to [d3-selection-multi](https://github.com/d3/d3-selection-multi#selection_styles).



<a href="#lineChunked_gapAttrs" name="lineChunked_gapAttrs">#</a> *lineChunked*.**gapAttrs**([*gapAttrs*])

Get or set *gapAttrs*, an object mapping attribute keys to attribute values to be applied only to undefined lines. It overrides values provided in *lineAttrs*. The passed in *gapAttrs* are merged with the defaults. Uses syntax similar to [d3-selection-multi](https://github.com/d3/d3-selection-multi#selection_attrs).

The default attrs are:

```js
{
  'stroke-dasharray': '2 2',
  'stroke-opacity': 0.2,
}
```


<a href="#lineChunked_pointStyles" name="lineChunked_pointStyles">#</a> *lineChunked*.**pointStyles**([*pointStyles*])

Get or set *pointStyles*, an object mapping style keys to style values to be applied to points. Uses syntax similar to [d3-selection-multi](https://github.com/d3/d3-selection-multi#selection_styles).



<a href="#lineChunked_pointAttrs" name="lineChunked_pointAttrs">#</a> *lineChunked*.**pointAttrs**([*pointAttrs*])

Get or set *pointAttrs*, an object mapping attr keys to attr values to be applied to points (circles). Note that if fill is not defined in *pointStyles* or *pointAttrs*, it will be read from the stroke color on the line itself. Uses syntax similar to [d3-selection-multi](https://github.com/d3/d3-selection-multi#selection_attrs).

