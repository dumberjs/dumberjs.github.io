---
layout: default
title: NODE_ENV
nav_order: 6
permalink: /node-env
---

# Conditional build based on NODE_ENV

`dumber` has built-in support of `NODE_ENV`. For example, if you use `NODE_ENV` in app code like this:

```js
if (process.env.NODE_ENV === 'production') {
  doSomething();
}
```

It will respect the `NODE_ENV` value captured at the time when the bundle files were created.

```bash
npx gulp build
env NODE_ENV=production npx gulp build
```

The two different builds will capture different `NODE_ENV` value in generated bundle files.

## Code removal

`dumber` not only captures the `NODE_ENV` value for app to consume at runtime, but it also pro-actively removes some conditional branch.

The above code snippet will be removed totally from final bundle when `NODE_ENV` is not production.

The code removal only works for very simple usage. It would not remove code for slightly more complex situations. For example:


```js
const env = process.env;
if (env.NODE_ENV === 'production') {
  doSomething();
}
```

Or
```js
if (aBoolean || process.env.NODE_ENV === 'production') {
  doSomething();
}
```

However it does work for simple if-else. One of the branches will be removed.

```js
if (process.env.NODE_ENV === 'production') {
  doSomething();
} else {
  doSomethingElse();
}
```
