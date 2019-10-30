---
layout: default
title: src
nav_order: 1
permalink: /options/src
parent: Options
---

# src

```js
src: 'src'
```

The default source path for local source files. Users unlikely need to change the default path `"src"`.

The src path affects module id that `dumber` assigned to each file. For example, `dumber` will assign module id `main` to file `src/main.js`, assign module id `foo/bar.html` to file `src/foo/bar.html`.