
# Mocha cloud GridView

  Terminal "grid" view for [mocha-cloud](https://github.com/visionmedia/mocha-cloud).

  ![mocha cloud testing](http://f.cl.ly/items/1e0C0L3t1x1u3w3J203u/mocha-grid-passes.png)
  ![mocha cloud failures](http://f.cl.ly/items/353m0I2j1w1p1g190i1g/mocha-grid-failures.png)

## Installation

    $ npm install mocha-cloud-grid-view

## Example

```js

/**
 * Module dependencies.
 */

var Canvas = require('term-canvas')
  , size = process.stdout.getWindowSize()
  , Cloud = require('mocha-cloud')
  , GridView = require('mocha-cloud-grid-view');

var cloud = new Cloud('project name', 'your username here', 'your access key here');

// the browsers to test

cloud.browser('iphone', '5.0', 'Mac 10.6');
cloud.browser('iphone', '5.1', 'Mac 10.8');
cloud.browser('iphone', '6', 'Mac 10.8');
cloud.browser('ipad', '5.1', 'Mac 10.8');
cloud.browser('ipad', '6', 'Mac 10.8');
cloud.browser('safari', '5', 'Mac 10.6');
cloud.browser('chrome', '', 'Mac 10.8');
cloud.browser('firefox', '15', 'Windows 2003');
cloud.browser('firefox', '16', 'Windows 2003');
cloud.browser('firefox', '17', 'Windows 2003');

// the local url to test

cloud.url('http://localhost:3000/test/');

// setup

var canvas = new Canvas(size[0], size[1]);
var ctx = canvas.getContext('2d');
var grid = new GridView(cloud, ctx);
grid.size(canvas.width, canvas.height);
ctx.hideCursor();

// trap SIGINT

process.on('SIGINT', function(){
  ctx.reset();
  process.nextTick(function(){
    process.exit();
  });
});

// output failure messages
// once complete, and exit > 0
// accordingly

cloud.start(function(){
  grid.showFailures();
  setTimeout(function(){
    ctx.showCursor();
    process.exit(grid.totalFailures());
  }, 100);
});

```

## License 

  MIT
