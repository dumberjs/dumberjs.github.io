---
layout: default
title: codeSplit
nav_order: 9
permalink: /options/code-split
parent: Options
---

# codeSplit

```js
codeSplit: function(moduleId, packageName) {
  // return a bundle name or nothing (default to entry bundle)
}
```

With the help from an AMD module loader, dumber-module-loader, the code split is intuitive and flexible in `dumber`.

`dumber` doesn't support special instructions in source code to tell bundlers how to split bundles (or so called chunks). `dumber` supports arbitrary code split regardless of the code.

The optional codeSplit callback is called for every single module, it takes two arguments:

1. moduleId:
  * For local src file "src/foo/bar.js", the module id is "foo/bar"
  * For local src file "src/foo/bar.css" (or any other non-js file), the module id is "foo/bar.css"
  * For npm package file "node_modules/foo/bar.js", the module id is "foo/bar"
2. packageName:
  * For any local src file, the package name is undefined
  * For npm package file "node_modules/foo/bar.js", the package name is "foo"
  * For npm package file "node_modules/@scoped/foo/bar.js", the package name is "@scoped/foo"

It should return a bundle name for current module, or return nothing which implicitly means the [entry bundle](./entry-bundle).

Here are few examples to show case the flexibility.

## Separate local files with npm packages.

1. Bundle all local sources in `app-bundle.js`.
2. Bundle all npm dependencies in `entry-bundle.js`.

```js
codeSplit: function(moduleId, packageName) {
  if (!packageName) return 'app-bundle';
}
```

## Separate local routes.

1. Bundle all local common files in `app-bundle.js`.
2. Assume the app has dedicated folders for every routes, bundle the routes into separate bundles, name after the route name.
3. Bundle all npm dependencies in `entry-bundle.js`.

```js
codeSplit: function(moduleId, packageName) {
  if (!packageName) {
    const parts = moduleId.split('/');
    // All src/foo/** to foo-bundle
    if (parts.length > 1) return parts[0] + '-bundle';
    return 'app-bundle';
  }
}
```

## Use dedicated bundles for few big npm dependencies.

1. Bundle all local sources in `app-bundle.js`.
2. Bundle codemirror npm package to `codemirror-bundle.js`.
2. Bundle all npm packages starts with "aurelia-" to `aurelia-bundle.js`.
4. Bundle the other npm dependencies in `entry-bundle.js`.

```js
codeSplit: function(moduleId, packageName) {
  if (!packageName) return 'app-bundle';
  if (packageName === 'codemirror') return 'codemirror-bundle';
  if (packageName.startsWith('aurelia-')) return 'aurelia-bundle';
}
```
