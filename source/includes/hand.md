# Hand

> Use require to get access to the preview.

```javascript
const hands = require('hands-preview');
```

The Hand API preview provides hand tracking data for supported platforms.

## Data

> Get the position and rotation of a specific joint on a specific hand.

```javascript
var pos = hands.getPosition(hands.hands.Right, hands.joints.Palm);
var rot = hands.getRotation(hands.hands.Right, hands.joints.Palm);
```

> Use `hands.None` if the handedness doesn't matter.

```javascript
var pos = hands.getPosition(hands.hands.None);
```

> All possible handedness values.

```javascript
hands.None
hands.Left
hands.Right
```

> All tracked joints.

```javascript
joints.Wrist
joints.Palm
joints.ThumbMetacarpalJoint
joints.ThumbProximalJoint
joints.ThumbDistalJoint
joints.ThumbTip
joints.IndexMetacarpal
joints.IndexKnuckle
joints.IndexMiddleJoint
joints.IndexDistalJoint
joints.IndexTip
joints.MiddleMetacarpal
joints.MiddleKnuckle
joints.MiddleMiddleJoint
joints.MiddleDistalJoint
joints.MiddleTip
joints.RingMetacarpal
joints.RingKnuckle
joints.RingMiddleJoint
joints.RingDistalJoint
joints.RingTip
joints.PinkyMetacarpal
joints.PinkyKnuckle
joints.PinkyMiddleJoint
joints.PinkyDistalJoint
joints.PinkyTip
```

Pose data can be read from the `hands` api using the getter functions.

## Events

> Subscribe and unsubscribe to events with `on` and `off`.

```javascript
hands.on('handdetected', function(handedness) {
    //
});

hands.off('handupdated', onHandUpdated);
```

> Complete list of events fired.

```javascript
handdetected
handupdated
handlost
```

Events are dispatched for hands entering and leaving detection. These can be useful for starting and stopping behaviors.