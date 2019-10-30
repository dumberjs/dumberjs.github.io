---
layout: default
title: Bundler Overview
nav_order: 3
permalink: /bundler-overview
---

# Bundler Overview

Let's start with a better understanding of bundler.

## Demystify bundling

Don't let those huge bundlers terrify you. The core business of a bundler is just doing two things:

1. Trace. Static analysis on code, find dependencies, bring them all in.
2. Pack. Concatenate files together.

Why they are huge? Because they added lots of features on top of the core business, like transpiling, dev server, minimization, ...

## Dumber is dumb
`dumber` does none of those, it's all about trace and pack, aka bundling.

The total code in core `dumber` is just 3.3K lines of JavasScript (includes [`dumber`](https://github.com/dumberjs/dumber), [`gulp-dumber`](https://github.com/dumberjs/gulp-dumber), [`dumber-module-loader`](https://github.com/dumberjs/dumber-module-loader), [`modify-code`](https://github.com/dumberjs/modify-code), and [`ast-matcher`](https://github.com/dumberjs/ast-matcher)). There are additional 200 lines in [`aurelia-deps-finder`](https://github.com/dumberjs/aurelia-deps-finder) to support classic [Aurelia](https://aurelia.io) in dumber.

`dumber` downgrades bundling to be a tiny step in gulp pipeline, and let gulp to take the centre stage. This humble design gained enormous flexibility with existing tools in gulp.

There is not much to learn about `dumber` because of the small feature set. Users need to learn [gulp](https://gulpjs.com). Gulp is the "shell". Your knowledge on gulp can be applied to the generic tasks beyond just bundling.

## No main (aka entry) module

Every other popular bundlers start bundling with a main module such as `src/main.ts`. They recursively trace and bring in all other files. It's inevitable for them to support transpiling, because they need to perform transpiling when bringing in new raw file.

`dumber` doesn't do transpiling. In the build task showed in previous page, you do proper transpiling on local files first, then feed them to `dumber`.

`dumber` only automatically brings in required npm package files. That assumes that all npm package files require no transpiling. For your local source files, `dumber` only double checks inner dependency. It warns you when a local dependency is not fulfilled (for example, `src/app.js` requires module `./foo`, but file `src/foo.js` was not fed to `dumber`).

> `dumber` is very passive on local source files. This design is to support runtime module requiring with least configuration. When a local dependency is missing, `dumber` only warns you but still goes ahead. The resulted bundle file(s) will have some missing modules. In app running time, [`dumber-module-loader`](https://github.com/dumberjs/dumber-module-loader) (an AMD module loader) will try to load those missing modules from remote. For example, you can ship an app with missing `client-config.json`, then supply a different `client-config.json` for every clients. We will explain more on this feature in later chapters.

In fact, `dumber` doesn't even need to know the entry module of the app.

1. Feed all the transpiled local sources files `src/**/*.{js,...}` to `dumber`.
2. `dumber` traces and packs all of them plus required npm packages.
3. To `dumber`, the entry module needs to be identified at app running time, but not bundling time.

The entry module can be marked in html file using traditional RequireJS way. By default, `dumber` treats `src/` as the local source folder, the module name for `src/main.js` is `main`, the module name for `src/foo/bar.js` is `foo/bar`.

Following example uses `src/main.js` as the entry module.

```html
<script src="dist/entry-bundle.js" data-main="main"></script>
```

Or you can explicitly start any module (or a list of modules).

```html
<script src="dist/entry-bundle.js"></script>
<script>requirejs(['main']);</script>
```

> Note: `dumber` uses [`dumber-module-loader`](https://github.com/dumberjs/dumber-module-loader). This AMD module loader inherited many RequireJS features and APIs. The two examples above use standard RequireJS feature which `dumber-module-loader` supports.

## No multi-versions of same npm package

This is an important difference between `dumber` and the other bundlers. `dumber` didn't exactly follow Nodejs module resolution, it doesn't support multiple versions of same npm package. When there is a version conflict in some depended npm package, `dumber` simply bundles the top level `node_modules/a-npm-package/` version which is the most common version in your app's dependencies tree.

This simplified approach is design decision with both Pros and Cons.
* Pros: `dumber` doesn't bundle duplicated npm package.
* Cons: `dumber` doesn't bundle duplicated npm package.

In our opinion, the Pros is more important to us.
