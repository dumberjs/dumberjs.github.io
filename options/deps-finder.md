---
layout: default
title: depsFinder
nav_order: 5
permalink: /options/deps-finder
parent: Options
---

# depsFinder

```js
depsFinder: null
```

`dumber` supports optional additional tracer for customised deps finding.

You can supply a function to find trace additional dependencies. Users are very unlikely to provide a customised deps finder.

```js
depsFinder: function(filePath, fileContents) {
  return ['array', 'of', 'additional', 'deps'];
  // Or return a promise to resolve to an array of strings.
}
```

The filePath is local file path, like `src/foo/bar.js` or `node_modules/foo/bar.js`.

The returned/resolved result array is strings of relative dependencies, like `["./foo.css", "a-npm-package", "a-npm-package/inner/file"]`.

## aurelia-deps-finder

`dumber` uses `aurelia-deps-finder` to support Aurelia conventions. Note this is only required for [classic Aurelia](https://github.com/aurelia/framework) app, not the upcoming [Aurelia 2](https://github.com/aurelia/aurelia).

```js
const dumber = require('gulp-dumber');
const auDepsFinder = require('aurelia-deps-finder');
const dr = dumber({
  depsFinder: auDepsFinder,
  // ...
});
```
