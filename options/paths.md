---
layout: default
title: paths
nav_order: 11
permalink: /options/paths
parent: Options
---

# paths

```js
paths: {
  'foo': 'another/module-id/for/foo'
}
```

Paths is an option for `dumber-module-loader`. `dumber` bundler passes the option to `dumber-module-loader` config.

It's similar to original RequireJS paths option, but simplified.

1. supports absolute path like `"foo": "/foo"`.
2. supports normal "foo": "common/foo", resolve `foo/bar/lo` to `common/foo/bar/lo`. Note we treat `common/foo/bar/lo` as the real module id, it could result to an existing known module, otherwise go through remote fetch.
3. doesn't support `"foo": ["common/foo", "shared/foo"]` the fail-over array.
4. relative module resolution is simplified. This is a breaking change from RequireJS. We changed the behaviour because we think our behaviour is less surprising.

Some explanation:

When you map `"foo"` to a relative path like `"another/foo"`, it treats the relative path as a module id. The module `"another/foo"` is either already bundled in bundle file, or will be fetched at runtime from remote.

The remote fetching of `"another/foo"` is relative to [baseUrl](./base-url) (`/dist` by default), so that it will fetch remote resource `/dist/another/foo.js` (appended `.js` since the module id has no file extension) at runtime.

When you map `"foo"` to a absolute path like `"/foo"`, the remote fetch will ignore baseUrl setting, and fetches `/foo.js` directly.

The module resolution breaking change is illustrated in the following example.

```js
define('common/foo', ['./bar'], function (bar) { /* ... */ });
requirejs.config({paths: {'foo': 'common/foo'}});
requirejs(['foo'], function (foo) {
  // Note the deep relative dependency on './bar':
  //
  // RequireJS resolves './bar' to 'bar', it respects the original 'foo'.
  //
  // dumber-module-loader resolves './bar' to 'common/bar', it respects
  // the real module id 'common/foo'.
});
```
