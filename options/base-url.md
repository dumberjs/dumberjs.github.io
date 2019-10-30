---
layout: default
title: baseUrl
nav_order: 2
permalink: /options/base-url
parent: Options
---

# baseUrl

```js
baseUrl: '/dist'
```

The baseUrl for remote AMD module loading, same as RequireJS option. dumber-module-loader consults the baseUrl at app running time to load additional bundle files (when code splitting is in use) and other missing modules.

Set baseUrl to the output directory of the bundle files. For example, if you write bundle files to `dist/` folder, set baseUrl to `/dist` (not the leading slash means from the root folder served by the web server).
