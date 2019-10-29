---
layout: default
title: Resources
nav_order: 4
description: Learn how resources are handled in dumber bundler
permalink: /resources
---

# Resources

Resources are the gulp vinyl files you throw to `dumber`.

Use again the example showed in [Get Started](get-started).

```js
function build() {
  return merge2(
    gulp.src('src/**/*.json'),
    gulp.src('src/**/*.html'),
    gulp.src('src/**/*.js').pipe(babel()),
    gulp.src('src/**/*.scss').pipe(sass())
  )
  // Here is dumber doing humble bundling.
  .pipe(dr())
  .pipe(gulp.dest('dist'));
}
```

Those json, html, js, css (gulp-sass turns scss vinyl file to css vinyl file) are resources.

`dumber` treats following resources specially:

## 1. js files

`dumber` transforms `.js` files into AMD modules.

`dumber` understands JavasScript code in either CommonJS, ESM (esnext), AMD, or UMD format.

`dumber` is very dumb, it doesn't understand `.ts` (TypeScript) or `.coffee` (CoffeeScript) files. You need to use gulp-typescript or gulp-coffee to transpile those files into `.js` vinyl files before sending to `dumber`.

Not required by `dumber`, but we recommend TypeScript users to turn on `esModuleInterop` to avoid possible headaches. https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-7.html#support-for-import-d-from-cjs-from-commonjs-modules-with---esmoduleinterop

## 2. wasm files

`dumber` wraps `.wasm` files into AMD modules. You can just use `import foo from './foo.wasm';` in your code.

wasm is bundled inside final JavaScript bundle file under base64 encoding.

## 3. json files

`dumber` wraps `.json` files into AMD modules.

For Nodejs compatibility, json modules are following Nodejs sematics, the result is something like `module.exports = { ... };`. Babel has no problem to understand it, but TypeScript users has to pick one of following two ways to support it.

1. Turn on `esModuleInterop`. https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-7.html#support-for-import-d-from-cjs-from-commonjs-modules-with---esmoduleinterop
2. Or use ESM (esnext) as the output module format (`"module": "esnext"` in `tsconfig.json`). With ESM as resulting format, `dumber` can normalise the import statements same as Babel.

## 4. css files

`dumber` wraps `.css` files into simple AMD text modules. By default, `dumber` also ships a css extension plugin for dumber-module-loader to inject the css into HTML head when you import the css file. User can opt-out the default behaviour with `injectCss: false`.

```js
const dr = dumber({
  injectCss: false,
  // ...
});
```

Similar to js files, `dumber` doesn't understand `.scss` or `.less` files. You need to use gulp-sass or gulp-less to transpile those files into `.css` vinyl files before sending to `dumber`.

If your source file is `foo.scss`, you can use either `import './foo.css';` or `import './foo.scss';`. Recommend you to use `import './foo.scss';`, as it's least surprising, and compatible with test frameworks Jest and AVA running in Nodejs environment.

The real bundled module id is `foo.css` as it is the only file `dumber` saw (after gulp-sass processed it). dumber-module-loader actually resolves module id `foo.scss` to `foo.css` at runtime.

## 5. all other files

All unknown files are treated as text modules. The includes html files.

This means `dumber` cannot bundle binary files like jpeg pictures. (`wasm` is the only binary format `dumber` accepts.)

## About gulp stream

In the gulp code sample, [merge2](https://github.com/teambition/merge2) is used to merge multiple gulp stream. Note gulp actually supports adding streams to pipeline, it was preprocessors (babel and sass) we need to isolate. Following code sample uses [gulp-if](https://github.com/robrich/gulp-if) instead of merge2 to do the same thing.

```js
function build() {
  return gulp.src('src/**/*.json')
    .pipe(gulp.src('src/**/*.html'))
    .pipe(gulp.src('src/**/*.js'))
    .pipe(gulpIf(file => file.extname === '.js', babel()))
    .pipe(gulp.src('src/**/*.scss'))
    .pipe(gulpIf(file => file.extname === '.scss', sass()))
    // Here is dumber doing humble bundling.
    .pipe(dr())
    .pipe(gulp.dest('dist'));
}
```

Or simply:

```js
function build() {
  return gulp.src('src/**/*.{json,html,js,scss}')
    .pipe(gulpIf(file => file.extname === '.js', babel()))
    .pipe(gulpIf(file => file.extname === '.scss', sass()))
    // Here is dumber doing humble bundling.
    .pipe(dr())
    .pipe(gulp.dest('dist'));
}
```
