---
layout: default
title: prepend and append
nav_order: 7
permalink: /options/prepend-and-append
parent: Options
---

# prepend and append

## prepend

```js
prepend: []
```

Refer to [entry bundle](./entry-bundle), prepend is an optional array of strings, to be prepended before booting up AMD module loader.

This option supports an aliased name "prepends".

You can supply either a local file path, or piece of code directly.

```js
prepend: [
  // Promise polyfill for IE
  "node_modules/promise-polyfill/dist/polyfill.min.js",
  "var my_global_var = { clientName: 'foo' };"
]
```

You can also conditionally prepend file or code. `dumber` ignores falsy values in prepend and append.

```js
prepend: [
  process.env.NODE_ENV === 'production' && "node_modules/promise-polyfill/dist/polyfill.min.js"
]
```

## append

```js
append: []
```
Refer to [entry bundle](./entry-bundle), append is an optional array of strings, to be appended after booting up AMD module loader and after RequireJS config.

This option supports an aliased name "appends".

It supports exactly same input as prepend.
