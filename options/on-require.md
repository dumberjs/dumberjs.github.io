---
layout: default
title: onRequire
nav_order: 10
permalink: /options/on-require
parent: Options
---

# onRequire

```js
onRequire: function(moduleId, parsedId) {
  // moduleId is a plain string of absolute module id, like
  // 'foo', 'text!bar/lo.html', '@scoped/name/dist/index'

  // parsedId is parsed object of the same moduleId, like
  // {prefix: 'text!', bareId: 'a/b.svg', parts: ['a', 'b.svg'], ext: '.svg', cleanId: 'text!a/b.svg'}
  // More details in https://github.com/dumberjs/dumber-module-loader/blob/619ead4c2d1380e0611daead3e7320bcfca6c8b6/test/id-utils.spec.js#L28-L62
}
```

An optional function to modify `dumber`'s tracing behaviour.

This option supports two aliased names "onrequire" and "onRequiringModule" (to be friendly to the users from aurelia-cli built-in bundler).

This optional callback is called before tracing any missing module. A missing module is something you didn't explicitly send to `dumber` through gulp pipeline or [explicit dependencies](./deps).

When this callback is absent, the default `dumber` behaviour is to try to find the missing module:
1. If the module id is a relative id from a local source file, complains that the user didn't provide through gulp pipeline (e.g. `local dependency foo (requiredBy app) is missing`).
2. Otherwise, try to find from node_modules, complains if cannot find (e.g. `cannot find npm package: foo`) and exits the build process. `dumber` does this for all npm packages which are not [defined explicitly](./deps) in the config.

Note, `dumber` does not attempt to directly load a local source file. This is designed to leave more freedom to the user, in order to control interesting runtime behaviour. It can finish the build process with a hole in local dependencies, but refuses to finish with a hole in npm dependencies.

The optional `onRequire` callback is designed to give the user more control over the tracing process.

It supports three types of result (all can be returned in promise):

1. **Boolean false**: ignore this module id;
2. **Array of strings like `['a', 'b']`**: ignore this module id, but trace module id `"a"` and `"b"` instead;
3. **A string**: the full JavaScript content of this module, must be in AMD format;
4. **All other returns are ignored** and fall back to the default tracing behaviour.

## 1. Ignore a module id at bundling time, supply it at runtime

For example, when building a multi-tenancy app (one code base to be deployed for multiple clients), you can ignore module id `"client-info.json"` (or `"client-info"` if you want to provide the config in js code) at bundling time, then fills it up at runtime.

First, use `onRequire` to calm `dumber` down on the missing module.

```js
onRequire: function(moduleId, parsedId) {
  // Return false to ignore client-info module
  if (moduleId === 'client-info.json') return false;
}
```

If you use TypeScript, also add a ts-ignore to calm TypeScript compiler down.

```js
// @ts-ignore TS2307
import clientInfo from 'client-info.json';
```

By default, `dumber-module-loader` will try to load module `"client-info.json"` from [baseUrl](./base-url) (e.g. `http://your-server/dist/client-info.json`) which is the same folder that all your bundle files were deployed. Create a customised `client-info.json` for every deployment, then the app will load the per client config with the help of runtime AMD module loader.

With other bundler, you can achieve the same thing without an AMD module loader. You can manually load a remote json file though `fetch` or ajax call. However, with the help of AMD module loader, there is less boilerplate and the module loading is written in "synchronous" import statement instead of asynchronous remote fetching.

> You may wonder how did AMD module loader make the asynchronous remote fetching works in synchronous semantics of the ESM import statement. The truth is that AMD module loader doesn't change the asynchronous nature of the remote fetching. The trick is that AMD module loader analyses the whole dependency tree, loads up all modules code (some synchronously and some asynchronously) but does not execute them yet. Only after all required dependencies are available, it then starts to execute the code. That's why when the module code runs, all dependencies can be resolved "synchronously".

> When you deliberately ignore a module, dumber-module-loader will fetch it at runtime. The runtime fetch of missing module is related to [baseUrl](./base-url) (`/dist` by default). You can also use [paths](./paths) option to alter the behaviour.

## 2. Bundling additional dependencies for the missing module

On top of the above use case, when the ignored module has additional dependencies of other npm packages, you might want to bundle those npm packages in your main app, so the runtime fetching of that ignored module would not trigger additional fetching on other npm packages.

To do this, return an array of dependencies.

```js
onRequire: function(moduleId, parsedId) {
  // ignore module 'foo', but bundle 'lodash' and 'jquery'
  // into the main app
  if (moduleId === 'foo') return ['lodash', 'jquery'];
}
```

## 3. Supply module implementation directly.

```js
onRequire: function(moduleId, parsedId) {
  // Supply code in AMD format
  if (moduleId === 'foo') return 'define(function() { return "foo"; });';
  // Or in CommonJS format
  if (moduleId === 'foo1') return 'module.exports = "foo1";';
  // Or in ESM format
  if (moduleId === 'foo2') return 'export default "foo2"';
}
```
