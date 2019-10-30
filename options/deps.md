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
