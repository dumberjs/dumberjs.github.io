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

`dumber` does only one thing (bundle resources), and does it well.

* **no transpiling**, because `gulp-babel` and `gulp-typescript` take care of that.
* **no minimization**, because `gulp-terser` takes care of that.
* **no dev server**, our scaffold uses [browser-sync](https://www.browsersync.io) as the dev server.
* **not even write file**, because `gulp.dest()` takes care of that.

One unique feature:

* a modern AMD module loader [`dumber-module-loader`](//github.com/dumberjs/dumber-module-loader).

Which enables:

* npm packages compatibility.
* flexible [code splitting](./options/code-split).
* runtime module requiring.
* runtime app composition.

## License

`dumber` is licensed under the [MIT license](https://github.com/makesjs/makes/blob/master/LICENSE).

## Acknowledgements

`dumber` used some code from [requirejs](https://github.com/requirejs/requirejs), [aurelia-pal](https://github.com/aurelia/pal), [aurelia-cli](https://github.com/aurelia/cli), [aurelia-templating-resources](https://github.com/aurelia/templating-resources), [style-loader](https://github.com/webpack-contrib/style-loader), and [node-libs-browser](https://github.com/webpack/node-libs-browser).
