# Behaviors

The entry point into scripting with Enklu is the `Behavior` script. Many `Behavior` scripts may be attached to elements throughout the hierarchy. They can manipulate the object graph, work with central systems, or send messages to scripts on other elements.


## Lifecycle Functions

> If an `enter` function is defined, it will be called once and only once after all scripts have been initially executed. This is a safe place to manipulate elements created through vines, as the elements are guaranteed to be created by this point.

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

> An `update` function may also be defined, which will be called every frame. This function is guaranteed to be called after `enter` is called, though an `enter` function is not required for `update` to be called.

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


## Scripts as Modules

> A simple script.

```javascript
log.debug('Hello World!');
```

> This script would be treated by the runtime like the following:

```javascript
var module_1 = { };
(function(module) {
    log.debug('Hello World!');
})(module_1);
```

> The runtime will use the specific module's exports to call any lifecycle functions as well any functions to expose to other scripts. Be sure to expose these methods via `module.exports` For example:

```javascript
function enter() {
    log.debug('Enter Called!');
}

function exit() {
    log.debug('Exit Called!');
}

function sayHi() {
    log.debug('Hello!');
}

module.exports = {
    enter: enter,
    exit: exit,
    sayHi: sayHi
};
```

All scripts are wrapped in functions to prevent name collision in the global scope. Because of this, all functions that should be exposed to other scripts and the [Lifecycle Functions](#lifecycle-functions) should be exported on a `module` object passed in by the runtime. 

