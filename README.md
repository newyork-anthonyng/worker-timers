# worker-timers

**A replacement for setInterval() and setTimeout() which works in unfocused windows.**

[![tests](https://img.shields.io/travis/chrisguttandin/worker-timers/master.svg?style=flat-square)](https://travis-ci.org/chrisguttandin/worker-timers)
[![dependencies](https://img.shields.io/david/chrisguttandin/worker-timers.svg?style=flat-square)](https://www.npmjs.com/package/worker-timers)
[![version](https://img.shields.io/npm/v/worker-timers.svg?style=flat-square)](https://www.npmjs.com/package/worker-timers)

## Motivation

For scripts that rely on [WindowTimers](http://www.w3.org/TR/html5/webappapis.html#timers) like
setInterval() or setTimeout() things get confusing when the site which the script is running on
loses focus. Chrome, Firefox and maybe others throttle the frequency of firing those timers to a
maximum of once per second in such a situation. However this is only true for the main thread and
does not affect the behavior of [Web Workers](http://www.w3.org/TR/workers/). Therefore it is
possible to avoid the throttling by using a worker to do the actual scheduling. This is exactly what
WorkerTimers do.

## Getting Started

WorkerTimers are available as a package on [npm](https://www.npmjs.org/package/worker-timers).
Simply run the following command to install it:

```shell
npm install worker-timers
```

You can then require the workerTimers instance from within your code like this:

```js
import * as workerTimers from 'worker-timers';
```

The usage is exactly the same as with the corresponding functions on the global scope.

```js
var intervalId = workerTimers.setInterval(() => {
    // do something many times
}, 100);

workerTimers.clearInterval(intervalId);

var timeoutId = workerTimers.setTimeout(() => {
    // do something once
}, 100);

workerTimers.clearTimeout(timeoutId);
```

## Server-Side Rendering

This package is intended to be used in the browser and requires the browser to have [support for
Web Workers](https://caniuse.com/#feat=webworkers). It does not contain any fallback which would it
allow to run in another environment like Node.js which doesn't know about Web Workers. This is to
prevent this package from silently failing in an unsupported browser. But it also means that it
needs to be replaced when used in a web project which also supports server-side rendering. That
should be easy, at least in theory, because each function has the exact same signature as its
corresponding builtin function. But the configuration of a real-life project can of course be
tricky. For a concrete example, please have a look at the
[worker-timers-ssr-example](https://github.com/newyork-anthonyng/worker-timers-ssr-example)
provided by [@newyork-anthonyng](https://github.com/newyork-anthonyng). It shows the usage inside
of a server-side rendered React app.
