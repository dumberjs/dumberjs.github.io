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

Note this changes the [module id](../resources#above-surface-module-id) assignment. File `node_modules/bar/index.js` will now be assigned with module id `foo/index.js` instead of `bar/index.js`.

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

