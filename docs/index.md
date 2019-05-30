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

Before future browsers + ESM (ECMAScript modules) bury all JavaScript bundlers, we still need a JavaScript bundler to ship SPA.

There is a trend in industry, a trend I dislike, to offer all-in-one solution. Almost every other bundlers do transpiling, minimization, dev server, and much more.

`dumber` is my attempt to follow the old school UNIX philosophy: make each program do one thing, and do it well.

* doesn't do transpiling. Because `gulp-babel` and `gulp-typescript` take care of that.
* doesn't do minimization. Because `gulp-terser` takes care of that.
* doesn't even write file! Because `gulp.dest()` takes care of that.
* doesn't provider dev server. Our scaffold uses [browser-sync](https://www.browsersync.io) as the dev server.

For other all-in-one bundlers, they are super easy to get started with demo and small scale app. But for every practical real world app, it's inevitable for developers to customise the build and deploy tasks. Once you hit the boundaries of all-in-one solutions, it becomes harder and harder to something they didn't offer.

`dumber` only does one thing, to pack js/html/css resources into js bundle files, no more and no less. By giving up so many unneeded features, `dumber` is simple but extremely flexible. By giving back control to the developer, `dumber` stays out your way. You write your build/deploy tasks in gulp, `dumber` is only one small step in your task `.pipe(dr())`.

Beside this design difference, there is one more unique feature that `dumber` offers.

**`dumber` uses AMD module format with a modern AMD loader [`dumber-module-loader`](//github.com/dumberjs/dumber-module-loader).** Beside intuitive code-splitting, this AMD module loader also enabled some interesting runtime possibilities that were very difficult for apps built with other bundlers. We will explore more in following pages.

## License

`dumber` is licensed under the [MIT license](https://github.com/makesjs/makes/blob/master/LICENSE).

## Acknowledgements

`dumber` used some code from [requirejs](https://github.com/requirejs/requirejs), [aurelia-pal](https://github.com/aurelia/pal), [aurelia-cli](https://github.com/aurelia/cli), [aurelia-templating-resources](https://github.com/aurelia/templating-resources), [style-loader](https://github.com/webpack-contrib/style-loader), and [node-libs-browser](https://github.com/webpack/node-libs-browser).
