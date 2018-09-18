# Gestures

```javascript
var gestures = require('gestures');
```

The Gestures API allows callbacks to be invoked when gestures are detected and lost.

<b>Note:</b> Gestures are only supported on the Hololens currently. Callbacks will not be called in the web editor.

## Events

```javascript
var pointerId;

function onPointerStarted(id) {
  log.info('New pointer detected: ', id);
  pointerId = id;
}

function onPointerEnded(id) {
  log.info('Pointer lost: ', id);
}

gestures.on('pointerstarted', onPointerStarted);
gestures.on('pointerended', onPointerEnded);
```

Gesture events can be listened for via `on` and `off`.

- `pointerstarted` - Dispatched when a new pointer is detected.
- `pointerended` - Dispatched when a pointer has been lost.

<b>Note:</b> The underlying gesture tracking may detect new pointers before an existing pointer has ended. It is recommended to use the most recent gesture id provided.

## Pose

```javascript
function update() {
  if (pointerId) {
    var pose = gestures.pose(pointerId);

    log.info('Pointer position:', pose.origin);
  }
}
```

Using a pointer id, the Gestures API can be queried for the latest pose data. Every field on each Pose will return either the current pointer data or a default value.

- `origin`    - The pointer's position in world space.
- `up`        - The pointer's up direction.
- `right`     - The pointer's right direction.
- `forward`   - The pointer's forward direction.
- `rotation`  - The pointer's rotation.
- `velocity`  - The pointer's linear velocity.

`pose` returns null if the pointer id is invalid.
