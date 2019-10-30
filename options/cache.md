---
layout: default
title: cache
nav_order: 3
permalink: /options/cache
parent: Options
---

# cache

```js
cache: true
```

`dumber` can cache tracing result to speed up consecutive runs. Cache is by default turned on.

You can conditionally turn off cache in some environment.

```js
cache: process.env.NODE_ENV !== 'production'
```