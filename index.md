---
layout: default
title: Home
nav_order: 1
description: A dumb JavasScript bundler for Single Page Application, dumber than you and me.
permalink: /
---

# dumber
{: .fs-10 }

A dumb JavasScript bundler for Single Page Application, dumber than you and me.
{: .fs-6 .fw-300 }

[Get Started](./get-started){: .btn .btn-blue .mr-3 .fs-5 } [View on Github](//github.com/dumberjs/dumber){: .btn .fs-5 }

---

`dumber` does only one thing, bundling resources, and does it well.

* **No transpiling**, because `gulp-babel` and `gulp-typescript` take care of that.
* **No minimization**, because `gulp-terser` takes care of that.
* **No dev server**, our [scaffold](https://github.com/dumberjs/new) uses [browser-sync](https://www.browsersync.io) as the dev server.
* **Not even writing file**, because `gulp.dest()` takes care of that.

One unique feature:

* A modern AMD module loader [`dumber-module-loader`](//github.com/dumberjs/dumber-module-loader).

Which enables:

* npm packages compatibility.
* Flexible [code splitting](./options/code-split).
* [Runtime module requiring](./options/on-require).
* [Runtime app composition](./runtime-app-composition).

## License

`dumber` is licensed under the [MIT license](https://github.com/makesjs/makes/blob/master/LICENSE).

## Acknowledgements

`dumber` used some code from [requirejs](https://github.com/requirejs/requirejs), [aurelia-pal](https://github.com/aurelia/pal), [aurelia-cli](https://github.com/aurelia/cli), [aurelia-templating-resources](https://github.com/aurelia/templating-resources), [style-loader](https://github.com/webpack-contrib/style-loader), and [node-libs-browser](https://github.com/webpack/node-libs-browser).
