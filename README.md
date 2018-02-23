# fusion-plugin-browser-performance-emitter

[![Build status](https://badge.buildkite.com/a7317a979159381e5e4ffb14e1ccd0d39737fd159f73863915.svg?branch=master)](https://buildkite.com/uberopensource/fusion-plugin-browser-performance-emitter)

The plugin emits events of performance stats from the browser - with the following API when avaliable:
(see https://developer.mozilla.org/en-US/docs/Web/API/Window/performance)
+ [Navigation Timing API](https://developer.mozilla.org/en-US/docs/Web/API/Navigation_timing_API)
[`Navigation Timing Processing Model`](https://www.w3.org/TR/navigation-timing/#processing-model)
![Navigation Timing Processing Model](https://www.w3.org/TR/navigation-timing/timing-overview.png)
+ [Resource Timing API](https://developer.mozilla.org/en-US/docs/Web/API/Window/performance)
[`Resource Timing Processing Model`](https://w3c.github.io/resource-timing/#processing-model)
![Resource Timing Processing Model](https://w3c.github.io/resource-timing/timestamp-diagram.svg)

On the server-side, it calculate performance opinionate **metrics** from the stats emitted from the browser, then re-emits a new event. Refer to [**Events**](#events) section for a list of events emitted.

---

### Table of contents

* [Installation](#installation)
* [Usage](#usage)
* [Setup](#setup)
* [API](#api)
  * [Registration API](#registration-api)
  * [Dependencies](#dependencies)
  * [Service API](#service-api)
* [Events](#events)
  * [Events listening to](#events-listening-to)
  * [Events emitted](#events-emitted)

---

### Installation

```sh
yarn add fusion-plugin-browser-performance-emitter
```

---

### Usage

```js
import {createPlugin} from 'fusion-core';
import {UniversalEventsToken} from 'fusion-plugin-universal-events';

export default createPlugin({
  deps: { emitter: UniversalEventsToken },
  provides: deps => {
    const emitter = deps.emitter;
    emitter.on('browser-performance-emitter:stats', e => {
      console.log(e); // log events to console
    });
  }
});
```

---

### Setup

```js
// src/main.js
import App from 'fusion-react';
import UniversalEvents, {UniversalEventsToken} from 'fusion-plugin-universal-events';
import BrowserPerformanceEmitter from 'fusion-plugin-browser-performance-emitter';

import PerformanceLogging from './performance-logging';

export default () => {
  const app = new App();
  // ...
  app.register(UniversalEventsToken, UniversalEvents);
  app.register(BrowserPerformanceEmitter);

  // (optional) a plugin to consume browser performance events
  app.register(PerformanceLogging);
  // ...
  return app;
}
```

---

### API

#### Registration API

##### `BrowserPerformanceEmitter`

```js
import BrowserPerformanceEmitter from 'fusion-plugin-browser-performance-emitter';
```

The browser performance emitter plugin. Typically, it doesn't need to be associated with a [token](https://github.com/fusionjs/fusion-core#token).

#### Dependencies

##### `UniversalEventsToken`

```js
import UniversalEvents, {UniversalEventsToken} from 'fusion-plugin-universal-events';

app.register(UniversalEventsToken, UniversalEvents);
```

An event emitter plugin to emit stats to, such as the one provided by [`fusion-plugin-universal-events`](https://github.com/fusionjs/fusion-plugin-universal-events).

#### Service API

This package has no public API methods. To consume performance events, add an event listener for the `browser-performance-emitter:stats` event on the server-side.

---

### Events

#### Events listening to

##### `browser-performance-emitter:stats:browser-only`

#### Events emitted

##### `browser-performance-emitter:stats`
