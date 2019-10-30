---
layout: default
title: entryBundle
nav_order: 4
permalink: /options/entry-bundle
parent: Options
---

# entryBundle

```js
entryBundle: 'entry-bundle'
```

Entry bundle is the default bundle name, it's `"entry-bundle"` by default. If no code splitting is in use, the entry bundle will be the only output bundle file. The default output file name is `entry-bundle.js` with `.js` file extension.

Note you don't need to add `.js` at the end of this option, but the extra `.js` would not confuse `dumber`.

## The composition of entry bundle

Because the entry bundle is the starting point of your app, there is some special structure inside to boot up AMD module loading.

1. Starts with optional prepends (another option to dumber).
  * All prepends contents are **before** initialising AMD loader.
  * You can load up legacy JavaScript libs in prepends.

2. Then dumber-module-loader brings up AMD module loader.
  * From this point onwards, global `define()`, `require()` and `requirejs()` functions are defined.
  * All code after this point are in AMD environment.

3. All user code modules and npm package modules in AMD module format.
  * `dumber` wraps all JavaScript modules in AMD module format.

4. A RequireJS config.
  * This config is important for enabling runtime remote bundle loading and remote module loading. It tells dumber-module-loader where to find additional bundle files or additional modules.
  * It inherited the same config method name from RequireJS.
```js
requirejs.config({
  baseUrl:'/dist',
  paths:{/* some path mappings including hashed bundle file names */},
  bundles:{/* additional bundles*/}
});
```

5. Ends with optional appends (another option to dumber).
  * All appends are **in** an AMD environment, because it's after booting up the AMD module loader.
  * You can use appends for some flexible customisation.

When code splitting is in use, other bundle files only contain part 3: user code modules and npm package modules in AMD module format.
