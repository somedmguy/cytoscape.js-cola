cytoscape-cola
================================================================================

[![DOI](https://zenodo.org/badge/42205998.svg)](https://zenodo.org/badge/latestdoi/42205998)


## Description

The Cola.js physics simulation layout for Cytoscape.js ([demo](https://cytoscape.github.io/cytoscape.js-cola))


The `cola` layout uses a [force-directed](http://en.wikipedia.org/wiki/Force-directed_graph_drawing) physics simulation with several sophisticated constraints, written by [Tim Dwyer](http://www.csse.monash.edu.au/~tdwyer/).  For more information about Cola and its parameters, refer to [its documentation](http://marvl.infotech.monash.edu/webcola/).

It supports noncompound and compound graphs well.

## Dependencies

* Cytoscape.js ^3.2.0
* Cola.js ^3.1.2


## Usage instructions

Download the library:
 * via npm: `npm install cytoscape-cola`,
 * via bower: `bower install cytoscape-cola`, or
 * via direct download in the repository (probably from a tag).

Import the library as appropriate for your project:

ES import:

```js
import cytoscape from 'cytoscape';
import cola from 'cytoscape-cola';

cytoscape.use( cola );
```

CommonJS require:

```js
let cytoscape = require('cytoscape');
let cola = require('cytoscape-cola');

cytoscape.use( cola ); // register extension
```

AMD:

```js
require(['cytoscape', 'cytoscape-cola'], function( cytoscape, cola ){
  cola( cytoscape ); // register extension
});
```

Plain HTML/JS has the extension registered for you automatically, because no `require()` is needed.



## API

Call the layout, e.g. `cy.layout({ name: 'cola', ... })`, with options:

```js
var defaults = {
  animate: true, // whether to show the layout as it's running
  refresh: 1, // number of ticks per frame; higher is faster but more jerky
  maxSimulationTime: 4000, // max length in ms to run the layout
  ungrabifyWhileSimulating: false, // so you can't drag nodes during layout
  fit: true, // on every layout reposition of nodes, fit the viewport
  padding: 30, // padding around the simulation
  boundingBox: undefined, // constrain layout bounds; { x1, y1, x2, y2 } or { x1, y1, w, h }
  nodeDimensionsIncludeLabels: undefined, // whether labels should be included in determining the space used by a node (default true)

  // layout event callbacks
  ready: function(){}, // on layoutready
  stop: function(){}, // on layoutstop

  // positioning options
  randomize: false, // use random node positions at beginning of layout
  avoidOverlap: true, // if true, prevents overlap of node bounding boxes
  handleDisconnected: true, // if true, avoids disconnected components from overlapping
  nodeSpacing: function( node ){ return 10; }, // extra spacing around nodes
  flow: undefined, // use DAG/tree flow layout if specified, e.g. { axis: 'y', minSeparation: 30 }
  alignment: undefined, // relative alignment constraints on nodes, e.g. function( node ){ return { x: 0, y: 1 } }
  gapInequalities: undefined, // list of inequality constraints for the gap between the nodes, e.g. [{"axis":"y", "left":node1, "right":node2, "gap":25}]. The constraint in the example says that the center of node1 must be at least 25 pixels above the center of node2. In other words, it is an inequality constraint that requires "node1.y + gap <= node2.y". You can set the extra "equality" attribute as "true" to convert it into an equality constraint.

  // different methods of specifying edge length
  // each can be a constant numerical value or a function like `function( edge ){ return 2; }`
  edgeLength: undefined, // sets edge length directly in simulation
  edgeSymDiffLength: undefined, // symmetric diff edge length in simulation
  edgeJaccardLength: undefined, // jaccard edge length in simulation

  // iterations of cola algorithm; uses default values on undefined
  unconstrIter: undefined, // unconstrained initial layout iterations
  userConstIter: undefined, // initial layout iterations with user-specified constraints
  allConstIter: undefined, // initial layout iterations with all constraints including non-overlap

  // infinite layout options
  infinite: false // overrides all other options for a forces-all-the-time mode
};
```


## Notes

- If you want to maintain interactivity, you probably should not mix `infinite: true` with `fit: true`.  Fitting naturally changes the zoom level, making dragging misaligned and feel weird to users --- though it still works technically.  Better to just `fit: false` when `infinite: true`, and `cy.center()` or `cy.fit()` on `layoutready`.
- The `alignment` option isn't as flexible as the raw Cola option.  Here, only integers can be used to specify relative positioning, so it's a bit limited.  If you'd like to see a more sophisticated implementation, please send a pull request.



## Build targets

* `npm run test` : Run Mocha tests in `./test`
* `npm run build` : Build `./src/**` into `cytoscape-cola.js`
* `npm run watch` : Automatically build on changes with live reloading (N.b. you must already have an HTTP server running)
* `npm run dev` : Automatically build on changes with live reloading with webpack dev server
* `npm run lint` : Run eslint on the source

N.b. all builds use babel, so modern ES features can be used in the `src`.


## Publishing instructions

This project is set up to automatically be published to npm and bower.  To publish:

1. Build the extension : `npm run build:release`
1. Commit the build : `git commit -am "Build for release"`
1. Bump the version number and tag: `npm version major|minor|patch`
1. Push to origin: `git push && git push --tags`
1. Publish to npm: `npm publish .`
1. If publishing to bower for the first time, you'll need to run `bower register cytoscape-cola https://github.com/cytoscape/cytoscape.js-cola.git`
1. [Make a new release](https://github.com/cytoscape/cytoscape.js-cola/releases/new) for Zenodo.
