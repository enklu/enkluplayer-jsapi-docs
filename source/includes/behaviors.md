# Behavior Scripts

## Overview

The entry point into scripting with Enklu is the `Behavior` script. These may be attached to elements throughout the hierarchy, end even communicate with each other.

## Lifecycle

```javascript
log.debug('Hello World!');
```

> An `update` function my also be defined, which will be called every frame.

```javascript
var acc = 0;
var radius = 1;
var speed = 1.3;

function update() {
    acc += speed * time.dt();

    this.transform.position = vec3(
        rad * Math.cos(acc),
        0,
        rad * Math.sin(acc));
}
```

A basic `Behavior` script does not have to do much at all, and is run as soon as it has been loaded.
