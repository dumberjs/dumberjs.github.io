---
layout: default
title: Get Started
nav_order: 2
description: Learn how to use dumber bundler
permalink: /get-started
---

# Get Started

Let's scaffold a front-end SPA project using dumber bundler. We use ["makes"](https://github.com/makesjs/makes) to help the scaffolding.

```bash
npx makes dumberjs
```

![makes dumberjs](https://makes.js.org/assets/makes-dumberjs.gif)

You will be prompted to provide a project name and few selections to generate a front-end project using `Aurelia`, `React`, or `Vue` framework.

Then you can start a dev server in the project with:
```sh
npm start
```

## Design of dumber

There is a trend in the industry to offer all-in-one solution. Almost every other bundlers do transpiling, minimization, dev server, and much more. I am not a fan of all-in-one solution.

`dumber` is my attempt to follow the old school UNIX philosophy: do one thing, and do it well. No transpiling, no minimization, no dev server...

For other all-in-one bundlers, they are super easy to get started with small app. But for every real world app, it's inevitable for developers to customise the build and deploy tasks. Once you hit the boundaries of all-in-one solutions, it becomes harder to do something they didn't offer.

`dumber` only does one thing, to pack resources into bundles. By giving up those unneeded features, `dumber` is simple yet flexible. By giving back control to you, `dumber` stays out of your way. You write your tasks in gulp, `dumber` is only one tiny step in your task `.pipe(dr())`.

## Quick example

Take an example, if you created an new project with `npx makes dumberjs new-project -s aurelia,sass`, your `tasks/build.js` is something similar to the following simplified snippet:

```js
function build() {
  return merge2(
    gulp.src('src/**/*.json'),
    gulp.src('src/**/*.html'),
    gulp.src('src/**/*.js').pipe(babel()),
    gulp.src('src/**/*.scss').pipe(sass())
  )
  .pipe(dr()) // Here is dumber doing humble bundling.
  .pipe(gulp.dest('dist'));
}
```

`dumber` swallows all incoming resources (js/json/html/css/wasm/...), then generates bundle files (new vinyl files, totally replaces the original vinyl files for original js/json/html/css/wasm/...). You can follow it up with `gulp.dest()` to write them out, you can do minimization between `.pipe(dr())` and `.pipe(gulp.dest('dist'))`.

1. `dumber` doesn't know how you prepare incoming resources. For example, we used `babel` to process `*.js` files, used `sass` to process `*.scss` files into `*.css` files.
  * this means `dumber` has zero code to support transpiling, because it doesn't need to try to know everything.
  * this humble design enables `dumber` with any preprocessing step. For example, you can use `gulp-coffee` to support CoffeeScript, `dumber` will only see `*.js` files compiled by CoffeeScript.
2. `dumber` doesn't care what you do with the bundle files. By default, it only generates one single bundle file `entry-bundle.js`, but you can turn on code splitting to produce multiple bundle files.
  * do minimization if you wish.
  * write files to any location, with optional source map (supported natively in gulp v4).

From the new project created by `npx makes dumberjs`, you can see we used few condition to control the behaviour of the build task. They are all implemented out of `dumber`. You need to learn how to customise tasks with gulp, you don't need to learn `dumber` very much because `dumber` doesn't provide many features (a virtue in our option).

## Start bundling

1. [Bundling Overview](bundling-overview)
2. TBD...
