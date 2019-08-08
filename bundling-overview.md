---
layout: default
title: Bundling Overview
nav_order: 3
description: Overview on bundling
permalink: /bundling-overview
---

# Bundling Overview

Before we show you various options of `dumber`, let's have a better overview on `dumber`'s unusual approach.

## Demystify bundling

Don't let those huge bundlers terrify you. The core business of a bundler is just doing two things:

1. Trace. Static analysis on code, find dependencies, bring them all in.
2. Pack. Concatenate files together.

Why those bundlers are huge? Because they added lots of features on top of the core business, like transpiling, dev server, minimization, ...

The total code in core `dumber` is just 3.3K lines of JavasScript (includes [`dumber`](https://github.com/dumberjs/dumber), [`gulp-dumber`](https://github.com/dumberjs/gulp-dumber), [`dumber-module-loader`](https://github.com/dumberjs/dumber-module-loader), [`modify-code`](https://github.com/dumberjs/modify-code), and [`ast-matcher`](https://github.com/dumberjs/ast-matcher)). There are additional 200 lines in [`aurelia-deps-finder`](https://github.com/dumberjs/aurelia-deps-finder) to support classic [Aurelia](https://aurelia.io) v1 in dumber. That's all it needs to trace and pack, aka bundling.

## No main (aka entry) module

Every other popular bundlers start bundling with a main module such as `src/main.ts`. They recursively trace and bring in all other files. It's inevitable for them to support transpiling, because they need to performing transpiling when bringing in new raw file.

`dumber` doesn't do transpiling. `dumber` doesn't bring in raw local file automatically. In the build task showed in previous page, you do proper transpiling on local files first, then feed them to `dumber`.

`dumber` only automatically brings in required npm package files. That assumes that all npm package files require no transpiling. For your local source files, `dumber` only double checks inner dependency. It warns you when a local dependency is not fulfilled (for example, `src/app.js` requires module `./foo`, but file `src/foo.js` was not fed to `dumber`).

> The reason of designing `dumber` very passive on local source files, is to support runtime module requiring with least configuration. When a local dependency is missing, `dumber` only warns you but still goes ahead. The resulted bundle file(s) will have some missing modules. At app running time, [`dumber-module-loader`](https://github.com/dumberjs/dumber-module-loader) will try to load those missing modules from remote. For example, you can ship an app with missing `client-config.json`, then supply a different `client-config.json` for every clients. We will explain more on this feature in later chapters.

In fact, `dumber` doesn't know, doesn't even need to know, your entry module.

1. You feed all your local sources files `src/**/*.{js,...}` to `dumber`.
2. `dumber` only traces and packs all of them plus required npm packages together.
3. To `dumber`, entry module is something needed to be known at app running time, not bundling time.

The entry module can be marked in html file using RequireJS way. By default, `dumber` treats `src/` as the base url of local modules, the module name for `src/main.ts` is `main`, the module name for `src/foo/bar.js` is `foo/bar`. Following example uses `src/main.ts` as the entry file.

```html
<script src="dist/entry-bundle.js" data-main="main"></script>
```

Or you can explicitly start to run any module (or a list of modules).

```html
<script src="dist/entry-bundle.js"></script>
<script>requirejs(['main']);</script>
```

> Note: `dumber` uses [`dumber-module-loader`](https://github.com/dumberjs/dumber-module-loader). This AMD module loader inherited many RequireJS features and APIs. The two examples above use standard RequireJS feature which `dumber-module-loader` supports.