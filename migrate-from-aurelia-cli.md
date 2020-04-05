---
layout: default
title: Migrate from aurelia-cli
nav_order: 8
permalink: /migrate-from-aurelia-cli
---

# Migrate from aurelia-cli

`dumber` was created as a total rewrite of the [`aurelia-cli`](https://github.com/aurelia/cli) built-in bundler.

`dumber` offers few advantages over the original `aurelia-cli` built-in bundler.

1. Not specific to Aurelia any more, `dumber` is a generic bundler.
2. Fixed compatibility with various npm packages which is impossible for `aurelia-cli` due to architecture flaw.
3. Greatly simplified configuration.

> The upcoming Aurelia 2 uses `dumber` as an alternative offering of webpack. `aurelia-cli` built-in bundler will be only maintained to support Aurelia 1.

> It's definitely not required for the users of `aurelia-cli` built-in bundler to migrate to `dumber`, as `aurelia-cli` built-in bundler will be continuously supported. However `dumber` does offer the above improvements that users might want to try out.

This guide highlights few key differences between `aurelia-cli` built-in bundler and `dumber` to help user to migrate their Aurelia 1 project over.

## Start a dumber Aurelia project

For Aurelia 1, use `npx makes dumberjs`, then choose "Aurelia".

For Aurelia 2, use official skeleton `npx makes aurelia`, then choose "Custom Aurelia 2 App", choose "Dumber".

## Configuration

`aurelia-cli` built-in bundler uses `aurelia_project/aurelia.json` to centralise configuration. It created quite a few confusion, plus non-intuitive condition syntax inside JSON.

`dumber` has no centralised configuration file. User configures everything directly in gulp tasks.

### Source files

In the gulp build task (`tasks/build.js` for Aurelia 1, or `gulpfile.js` for Aurelia 2), user can modify the source files directly.

```js
function build() {
  return merge2(
    gulp.src('src/**/*.json', {since: gulp.lastRun(build)}),
    gulp.src('src/**/*.html', {since: gulp.lastRun(build)}),
    buildJs('src/**/*.ts'),
    buildCss('src/**/*.css')
  )
  //...
}
```

### Conditional logic

`aurelia-cli`'s `aurelia_project/aurelia.json` has some condition logic like:

```
"minify": "stage & prod",
```

And
```
{
  "name": "aurelia-testing",
  "env": "dev"
},
```

`dumber` doesn't need them. User directly uses code to control the logic in gulp tasks.

```js
.pipe(gulpif(isProduction, terser({compress: false})))
```

```js
dumber({
  deps: [
    // falsy value is ignored in deps array.
    process.env.NODE_ENV !== 'production' && 'any-npm-package-name',
  ]
  // ...
});
```

Note you actually doesn't need to use the above conditional dependency to support `aurelia-testing`. `dumber` understands `NODE_ENV`, user can directly do conditional dependency in code. See [`NODE_ENV`](./node-env) for more details.

```js
if (process.env.NODE_ENV === 'production') {
  aurelia.use.plugin('aurelia-testing');
}
```

> Note, same as `aurelia-cli` built-in bundler, `dumber` understands all Aurelia conventions (through package [`aurelia-deps-finder`](https://github.com/dumberjs/aurelia-deps-finder)). Users do not need to use `PLATFORM.moduleName()` wrapper.

### Entry bundle file name

The default entry bundle file name is now "entry-bundle.js", not "vendor-bundle.js". See [entry-bundle](./options/entry-bundle) for more details.

### Prepend and append

"prepend" and "append" are now configured on dumber bundler instance, not on the entry bundle. See [prepend-and-append](./options/prepend-and-append) for more details.

### Explicit dependencies

"deps" (or "dependencies") is now configured on dumber bundler instance, not on the entry bundle.

The shape of dependency config also changed.

Previously in `aurelia-cli`:

```json
{
  "name": "npm-package-name",
  "path": "../node-modules/npm-package-path", // optional
  "main": "main/file.js" // optional
}
```

It's now:

```js
{
  name: 'npm-package-name',
  location: 'node-modules/npm-package-path', // optional
  main: 'main/file.js' // optional
}
```

The "path" has been changed to "location", this is to match original `RequireJS` configuration. Also, the "location" is relative to project root folder, not the project "src/" folder as the "path" related to.

Previously `aurelia-cli` also support `"path": "../node-modules/path/and/main.js"`. `dumber` doesn't support the merged file path, user needs both "location" and "main".

See [deps](./options/deps) for more details.

### Code split

`aurelia-cli` had very tedious bundles configuration, `dumber` reduced it to a simple callback. See [codeSplit](./options/code-split) for more details.

### Hashed bundle file name

For updating `index.html` with hashed bundle file name, see [hash and onManifest](./opions/hash-and-on-manifest) for more details.

