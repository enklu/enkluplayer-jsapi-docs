# Gaze

The Gaze API provides eye tracking details for supported platforms.

```javascript
const gaze = require('gaze-preview');
```

## Gaze Types

There are two types of gaze: glance and focus. Glance is used to determine if an element has _ever_ been looked at, while focus is used when a user's eyes focus on an element. Both types can be refered to via the `gaze.types` object.

## Registration

> Events of this type will now be dispatched for this element.

```javascript
var self = this;
gaze.register(self, gaze.types.Focus);
```

Before we can capture gaze events, we need to register elements for gaze tracking.

## Events

> These events are dispatched when a glance has started.

```javascript
gaze.on('glancestarted', onGlanceStarted);
```

> These events are dispatched every frame a glance is updated.

```javascript
gaze.on('glanceupdated', onGlanceUpdated);
```

> These events are dispatched when a glance has stopped.

```javascript
gaze.on('glancestopped', onGlanceStoped);
```

> These events are dispatched when a focus has started.

```javascript
gaze.on('focusstarted', onFocusStarted);
```

> These events are dispatched every frame a focus has updated.

```javascript
gaze.on('focusupdated', onFocusUpdated);
```

> These events are dispatched when a glance has stopped.

```javascript
gaze.on('focusstopped', onFocusStopped);
```

> Each of these handlers take nothing, but instead poll the gaze object for state.

```javascript
function onFocusUpdated() {
    var dir = gaze.glanceDirection;
    
    //
}
```

> Handlers may be unregistered with `off`.

```javascript
gaze.off('focusstopped');
```

Gaze events are fired direcly from the `gaze` interface, just like any other event dispatcher.