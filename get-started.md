---
layout: default
title: Get Started
nav_order: 2
permalink: /get-started
---

# Get Started

Let's scaffold a JavaScript SPA project. We use the ["makes"](https://github.com/makesjs/makes) tool with zero installation.

```bash
npx makes dumberjs
```

![makes dumberjs](https://makes.js.org/assets/makes-dumberjs.gif)

You will be prompted to provide a project name and few selections to generate a front-end project using `Aurelia`, `React`, or `Vue` framework.

After install npm dependencies, you can start a dev server in the project with:
```sh
npm start
```

## Design of dumber

Almost all other bundlers do transpiling, minimization, dev server, and much more.

`dumber` is an attempt to follow the old school UNIX philosophy: do one thing, and do it well. Just bundling, no transpiling, no minimization, no dev server...

For other all-in-one bundlers, they are super easy to get started with small app. But for any real world app, it's inevitable for the developers to customise the build and deploy tasks. The all-in-one design creates boundaries for integration, normally means, for any new feature, user needs a dedicated plugin built for this bundler. Advanced users could also experience hardship when they need something the all-in-one didn't offer.

`dumber` only does one thing, to pack resources into js bundles. By giving up those features, `dumber` is simple yet flexible. By giving back control to the user, `dumber` stays out of the way. Users write the tasks in gulp, `dumber` is only one tiny step `.pipe(dr())`.

## Quick example

Take an example, if you created an new project with `npx makes dumberjs new-project -s aurelia,sass`, your `tasks/build.js` is logically similar to the following simplified snippet:

```js
const dumber = require('gulp-dumber');
const dr = dumber({/* ... */});

function build() {
  return merge2(
    gulp.src('src/**/*.json'),
    gulp.src('src/**/*.html'),
    gulp.src('src/**/*.js').pipe(babel()),
    gulp.src('src/**/*.scss').pipe(sass())
  )
  // Here is dumber doing humble bundling.
  // The extra call `dr()` is designed to cater watch mode.
  .pipe(dr())
  .pipe(gulp.dest('dist'));
}
```

`dumber` swallows all incoming resources (js/json/html/css/wasm/...), then generates js bundle files (new vinyl files, totally replaces the original vinyl files from original js/json/html/css/wasm/...). You can follow up with `gulp.dest()` to write them out, and any other steps before `gulp.dest` (for instance, minimization).

1. `dumber` doesn't know how you prepare incoming resources. For example, we used `babel` to process `*.js` files, used `sass` to process `*.scss` files into `*.css` files.
  * `dumber` has zero code to support transpiling or plugin, because it doesn't need to.
  * This humble design enables `dumber` to work with any preprocessing step. For example, you can use `gulp-coffee` to support CoffeeScript, `dumber` will only see `*.js` files compiled by CoffeeScript.
2. `dumber` doesn't care about what you do with the bundle files.
  * By default, it only generates one single bundle file `entry-bundle.js`, but you can turn on code splitting to produce multiple bundle files.
  * Do minimization if you wish.
  * Write files to any location, with optional source map (supported natively in gulp v4).

## Start bundling

1. [Bundler Overview](./bundler-overview)
2. [Resources](./resources)
3. [Options](./options)
4. [NODE_ENV](./node-env)
5. [Runtime App Composition](./runtime-app-composition)
