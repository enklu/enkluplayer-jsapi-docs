# Touch

The `touch` system is a higher-level system that combines elements and gestures. It is useful for registering for determining if a user touches an element with their finger.

> This API is available only on HoloLens.

## Registration

```javascript
var touch = require('touch');
touch.register(this);

// some time later

touch.unregister(this);
```

> Currently, touch events are only supported on asset elements.

Registering an element for touch events is very simple. Simply require the touch system and call register.

## Events

```javascript
touch.register(this);

// dispatched when a touch has started
this.on('touchstarted', function(hit) {

});

// dispatched after a touch has been started but before it has been stopped
this.on('touchdragged', function(hit) {

});

// dispatched when a touch has stopped
this.on('touchstopped', function() {

});
```

Registering an element with the `touch` system will cause that element to dispatch events when touched. These can be listened to just like any other event dispatched from an element: with `on`.

```javascript
function onHit(hit) {
  log.info('Touch detected!');
  log.info('  Position: ' + hit.position);
  log.info('  Normal: ' + hit.normal);
}
```

The payload returned by a touch callback will contain the `position` and `normal` of the hit.

> Check for successful registration.

```javascript
var success = touch.register(this.findOne('..(@type=LightWidget)'));
if (!success) {
  log.warn('Unable to register Element for touch!');
}
```

The underlying API will reject registrations from non-asset Elements.
