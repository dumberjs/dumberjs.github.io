---
layout: default
title: hash and onManifest
nav_order: 6
permalink: /options/hash-and-on-manifest
parent: Options
---

# hash and onManifest

## hash

```js
hash: false
```

`dumber` can add hash to bundle file names. It's by default off, user should turn it on for production build.

```js
hash: process.env.NODE_ENV === 'production'
```

`dumber` implemented this feature instead of leaving it to gulp-hash. The reason is that RequireJS config (in the end of entry bundle file) needs to know the exact bundle file names.

When turned on, a bundle file name `entry-bundle.js` will become `entry-bundle.01af123b1f13a51c4c8b458656da47b3.js`.

## onManifest callback

When hashed bundle files were generated, user needs to somehow update the app index html page with the latest entry bundle file name. This is where onManifest callback is called.

```js
onManifest: function(filenameMap) {
  // filenameMap is like:
  // {
  //   "entry-bundle.js": "entry-bundle.1234.js",
  //   "other-bundle.js": "other-bundle.3455.js"
  // }

  // Update index.html entry-bundle.js with entry-bundle.12345.js
  console.log('Update index.html with ' + filenameMap['entry-bundle.js']);
  const indexHtml = fs.readFileSync('_index.html').toString()
    .replace('entry-bundle.js', filenameMap['entry-bundle.js']);
  fs.writeFileSync('index.html', indexHtml);
}
```

The above example use `_index.html` as the template for the resulting `index.html` file.
The updated `index.html` file needs to be deployed along with all JavaScript bundle files.
