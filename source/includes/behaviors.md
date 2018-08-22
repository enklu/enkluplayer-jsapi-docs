# Behaviors

## Overview

The entry point into scripting with Enklu is the `Behavior` script. Many `Behavior` scripts may be attached to elements throughout the hierarchy. They can manipulate the object graph, work with central systems, or send messages to scripts on other elements.

## Lifecycle

> A simple script.

```javascript
log.debug('Hello World!');
```

> If an `enter` function is defined, it will be called once after all scripts have been executed initially. This is a safe place to manipulate elements created through vines, as the elements are guaranteed to be created at this point.

```javascript
var okBtn;

function enter() {
	okBtn = this.find('..btn-ok');
	okBtn.on('activated', onOkActivated);
}

function onOkActivated() {
	log.info('Ok button was activated!');
}
```

> An `update` function may also be defined, which will be called every frame after `enter` is called. An `enter` function does not need to be specified for an `update` function to be called.

```javascript
var acc = 0;
var radius = 1;
var speed = 1.3;

function update() {
    acc += speed * time.dt();

    // move the object in a circle
    this.transform.position = vec3(
        rad * Math.cos(acc),
        0,
        rad * Math.sin(acc));
}
```

> Finally, an `exit` function may be defined which is called right before the object is destroyed.

```javascript
function exit() {
	okBtn.off('activated', onOkActivated);
}
```

Before any `Vine` or `Behavior` scripts are executed, all scripts on an element are loaded. Once they are all loaded, they begin to execute in the order in which they are arranged on the element.

After initial execution of all `Vine` and `Behavior` scripts, each `Behavior` script will begin calling lifecycle methods.