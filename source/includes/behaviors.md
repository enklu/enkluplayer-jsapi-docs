# Behavior Scripts

## Overview

The entry point into scripting with Enklu is the `Behavior` script. These may be attached to elements throughout the hierarchy, end even communicate with each other.

## Lifecycle

```javascript
log.debug('Hello World!');
```

A basic `Behavior` script does not have to do much at all, and is run as soon as it has been loaded.

```javascript
var acc = 0;

function update() {
    acc += time.dt();
}
```

An `update` function my also be defined, which will be called every frame.
