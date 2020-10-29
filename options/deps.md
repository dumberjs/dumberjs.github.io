---
layout: default
title: deps
nav_order: 8
permalink: /options/deps
parent: Options
---

# deps

```js
deps: []
```

An optional array of dependencies. User can use this option to forcefully add explicit dependencies, or override default location or main file of a npm package.

This option supports an aliased name "dependencies".

# Supported dependency input

## A simple npm package name

```js
deps: ['jquery']
```

This forcefully bundles jquery npm package even when the user code didn't explicit import it.

## A package name with modified location

```js
deps: [ {name: 'jquery', location: '../my-local-jquery-fork'} ]
```

This will bundle jquery package from a locale folder relative to the project root.

## A package name with modified main

```js
deps: [ {name: 'jquery', main: 'dist/jquery.min.js'} ]
```

This will use jquery package's `dist/jquery.min.js` as the main file instead of `dist/jquery.js` set by jquery `package.json` main field.

Note, `main` and `location` can be used together.

## Package alias

Alias is just a flexible use case of "a package name with modified location".

```js
deps: [ {name: 'foo', location: 'node_modules/bar'} ]
```

This will use npm package `bar` as if it's `foo`.

Note this changes the [module id](../resources#module-id) assignment. File `node_modules/bar/index.js` will now be assigned with module id `foo/index.js` instead of `bar/index.js`.

## Conditional dependency

Similar to [prepend and append](./prepend-and-append), `dumber` ignores falsy values in deps.

```js
deps: [
 process.env.NODE_ENV !== 'production' && 'aurelia-testing',
 process.env.NODE_ENV !== 'production' && {
   name: 'foo',
   location: '../local/foo'
 }
]
```

## Lazy Main

By default, explicit dependencies force `dumber` to bundle its main file. This is usually what you want.

By sometimes, your code didn't use the main file but some other file in the npm package. For example, your code only uses `lodash/map` (which resolves to `map.js` in `lodash` package), but you have a special modified lodash package.

```js
deps: [
  {name: 'lodash', location: '../my-lodash'}
]
```

This explicit dependency will force `dumber` not only bundle `map.js` of `lodash`, but all other files that `lodash` main file requires.

To skip the main file and only bundle `lodash/map`, use `lazyMain: true`.

```js
deps: [
  {name: 'lodash', location: '../my-lodash', lazyMain: true}
]
```

> Note if you had this lazyMain dep in `dumber` config, but didn't require any `lodash` module (`lodash` or `lodash/some-module`) in your code, `dumber` would not bundle any `lodash` file.

## Shim

For traditional JavaScript libraries that do not support any module format (AMD, UMD, CommonJS or ESM). We can use shim to wrap it into an AMD module.

> This is very rare in current JavaScript landscape. Popular JavaScript libraries all support one of the module formats. There is no need of shim configuration.

If you still need to use some outdated legacy JavaScript libraries, you can use [RequireJS shim confugration](https://requirejs.org/docs/api.html#config-shim) to wrap it.

Three options in shim configuration:
* `deps` (optional) an array of strings. This is an array of dependencies which must be loaded and available before the legacy library can be evaluated.
* `exports` (optional) string. This is the name of the global variable that should be used as the exported value of the module.
  - for example "exports: 'lorem'" will set `window.lorem` with this module value.
* `wrapShim` (optional, try this if normal shim does not work) a bookean.  wrap the legacy code in a function. This will delay the execution of the legacy code to module loading time.

Take [`tempusdominus-bootstrap-4`](https://github.com/tempusdominus/bootstrap-4) as an example, it uses browser globals instead of any module format.

```js
deps: [
  {
    "name": "tempusdominus-bootstrap-4",
    "deps": [
      "jquery",
      "bootstrap",
      "popper.js",
      "moment"
    ],
    "wrapShim": true
  }
]
```

So that user can do `import 'tempusdominus-bootstrap-4';` in code.

Shim configuration could be confusion for users not familiar with AMD module format. When you work with legacy JavaScript libraries, the other easier alternative is to use [prepend](./prepend-and-append).

Prepend puts the JavaScript libraries before AMD module loader kicked in, they behaves like `<script>` tags before entry-bundle file.

For example, you can either use prepend:

```js
prepend: [
  require.resolve('jquery'),
  require.resolve('popper.js'),
  require.resolve('bootstrap'),
  require.resolve('moment'),
  require.resolve('tempusdominus-bootstrap-4')
],
```

Or just use `<script>` tags:

```html
<script src="/path/to/jquery.js"></script>
<script src="/path/to/popper.js"></script>
<script src="/path/to/bootstrap.js"></script>
<script src="/path/to/moment.js"></script>
<script src="/path/to/tempusdominus-bootstrap-4.js"></script>

<script src="/dist/entry-bundle.js" data-main="main"></script>
```