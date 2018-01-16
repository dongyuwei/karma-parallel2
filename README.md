# karma-parallel

[![npm version](https://img.shields.io/npm/v/karma-parallel.svg?style=flat-square)](https://www.npmjs.com/package/karma-parallel)
[![npm downloads](https://img.shields.io/npm/dm/karma-parallel.svg?style=flat-square)](https://www.npmjs.com/package/karma-parallel)
[![js-standard-style](https://img.shields.io/badge/code%20style-standard-brightgreen.svg?style=flat-square)](https://github.com/joeljeske/karma-parallel)
[![dependencies Status](https://david-dm.org/joeljeske/karma-parallel/status.svg)](https://david-dm.org/joeljeske/karma-parallel)
[![devDependencies Status](https://david-dm.org/joeljeske/karma-parallel/dev-status.svg)](https://david-dm.org/joeljeske/karma-parallel?type=dev)

> A Karma JS plugin to support sharding tests to run in parallel across multiple browsers.

# Overview

This is intended to speed up the time it takes to run unit tests by taking advantage of multiple cores. From a single
karma server, multiple instances of a browser are spun up. Each browser downloads all of the spec files, but when a 
`describe` block is encountered, the browsers deterministically decide if that block should run in the given browser. 

This leads to a way to split up unit tests across multiple browsers without changing any build processes. 

## Installation

The easiest way is to install `karma-parallel` as a `devDependency`.

**Using NPM**

```bash
npm install karma-parallel --save-dev
```

**Using Yarn**

```bash
yarn add karma-parallel --dev
```


## Examples

### Basic

```javascript
// karma.conf.js
module.exports = function(config) {
  config.set({

    frameworks: ['mocha' /* or 'jasmine' */, 'parallel'], // this will load the framework and beforeMiddleware
    
    parallelOptions: {
      executors: 4,
      shardStrategy: 'round-robin' /* or 'description-length' */
    }
  });
};
```


## Options

`parallelOptions [object]`: Options for this plugin

`parallelOptions.executors [int=cpu_cores-1]`: The number of browser instances to
use to test. If you test on multiple types of browsers, this spin up the number of 
executors for each browser type. 

`parallelOptions.shardStyle [string='round-robin']`: This plugin works by 
overriding the test suite `describe()` function. When it encounters a describe, it 
must decide if it will skip the tests inside of it, or not. 

* The `round-robin` style will only take every `executors` test suite and skip the ones in between.
* The `description-length` deterministically checks the length of the description for each test suite use a modulo of the number of executors. 



----

For more information on Karma see the [homepage].

## See Also

[`karma-sharding`](https://github.com/rschuft/karma-sharding)

This similar project works by splitting up the actual spec files across the browser instances. 

Pros: 

* Reduces memory by only loading *some* of the spec files in each browser instance

Cons: 

* Requires the spec files to reside in separate files, meaning it is not compatible with bundlers such 
as [`karma-webpack`](https://github.com/webpack-contrib/karma-webpack) or [`karma-browserify`](https://github.com/nikku/karma-browserify)



[homepage]: http://karma-runner.github.com

