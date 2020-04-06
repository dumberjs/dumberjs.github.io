---
layout: default
title: CSS library
nav_order: 1
permalink: /common-recipes/css-library
parent: Common recipes
---

# CSS library

There are few ways to uses a CSS library inside your app.

## Use CDN directly

Directly add the CSS into `_index.html` head section.

```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.13.0/css/all.min.css" integrity="sha256-h20CPZ0QyXlBuAw7A+KluUYx/3pK+c7lYEpqLTlxjYQ=" crossorigin="anonymous" />
```

> Note the `npx makes dumberjs` created app uses `_index.html` as a template for the final `index.html`. The gulp build task will write hashed bundle file name into `index.html`. User needs to modify the original `_index.html` file.

This simple solution has few advantages:
1. reduce complexity of local app build.
2. it just works with additional resources like images and fonts required by the CSS.
3. CDN offers great performance from the CDN network.

The the above reasons, this direct usage over CDN is the recommended solution to support any CSS library.

For some rare occasions, for example a corporate internal app which has no access to general internet, direct load from CDN is not an option.

## Import locally installed npm package

Take [normalize.css](https://github.com/necolas/normalize.css) as example.

```bash
npm install normalize.css
```

Then in your app:

```js
import 'normalize.css';
// Or explicitly load the main file
// import 'normalize.css/normalize.css';
```

`dumber` will bundle the css and load it into html head at runtime. For CSS library without additional resources (images or fonts), the above is all you need to do.

### For additional resources

Take [font awesome 5](https://github.com/FortAwesome/Font-Awesome) as example.

```bash
npm install @fortawesome/fontawesome-free
```

```js
import '@fortawesome/fontawesome-free/css/all.min.css';
```

However, this is not enough, because fontawesome needs fonts at runtime. You need to make sure the fonts are in correct folders in public web server.

`dumber` does nothing about this, users need to use gulp to copy files.

```js
function copyFonts() {
  return gulp.src('node_modules/@fortawesome/fontawesome-free/webfonts/*')
    .pipe(gulp.dest('@fortawesome/fontawesome-free/webfonts/'));
}
```

> Note you need to deploy `index.html`, `dist/` and `@fortawesome/` folders to production server.

[Demo app](https://github.com/dumberjs/examples/tree/master/demo-copy-fonts).
