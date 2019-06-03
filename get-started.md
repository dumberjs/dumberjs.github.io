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

![makes dumberjs](https://makesjs.github.io/assets/makes-dumberjs.gif)

You will be prompted to provide a project name and few selections to generate a front-end project using `Aurelia`, `React`, or `Vue` framework.

Then you can start a dev server in the project with:
```sh
npm start
```

## Design of dumber

There is a trend in industry, a trend I dislike, to offer all-in-one solution. Almost every other bundlers do transpiling, minimization, dev server, and much more.

`dumber` is my attempt to follow the old school UNIX philosophy: do one thing, and do it well. No transpiling, no minimization, no dev server...

For other all-in-one bundlers, they are super easy to get started with small app. But for every real world app, it's inevitable for developers to customise the build and deploy tasks. Once you hit the boundaries of all-in-one solutions, it becomes harder and harder to do something they didn't offer.

`dumber` only does one thing, to pack resources into bundles. By giving up those unneeded features, `dumber` is simple yet flexible. By giving back control to you, `dumber` stays out of your way. You write your tasks in gulp, `dumber` is only one small step in your task `.pipe(dr())`.


