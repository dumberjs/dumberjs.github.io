---
layout: default
title: Options
nav_order: 5
permalink: /options
has_children: true
---

# Options

gulp-dumber supports various options. You can modify the options in your local `gulpfile.js`.

```js
const dumber = require('gulp-dumber');
const dr = dumber({
  // src folder is by default "src".
  src: 'src',
  // Other options
});
```
