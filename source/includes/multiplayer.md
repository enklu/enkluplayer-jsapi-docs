# Multiplayer

> The multiplayer system may be accessed via the `mp` require.

```javascript
var mp = require('mp);
```

> To control synchronization between clients, first obtain a context for the element in question. All synchronization for a specific element is directed through this context object.

```javascript
var ctx = mp.context(el);
```

Enklu comes with a specialized interface for realtime multiplayer. Currently, the player will automatically join a multiplayer server instance for the specific experience they are loading into. The multiplayer js api is constructed such that it is transparent to the client whether or not it is connected. This means that scripts do not have to contain their own complicated logic for multiplayer fallbacks.

At a high level, the multiplayer system can synchronize elements and their schema properties across clients. This means that when a client changes the position of an element, for instance, the change will be propagated to all clients so that the position prop stays in sync.

## Auto-Toggle

> Here is a complete, annotated code example.

```javascript
var self = this;
var el;

function enter() {
    // find a specific child element
    el = self.findOne('..bar');
    
    // Watch for the 'onPlay' prop to change-- this will be called if we change the prop
    // OR if anyone else changes the prop using the autoToggle function (see onTouched()).
    el.schema.watchBool('onPlay', function(prev, next) {
        if (next) {
            // play animation
        }
    });
}

function onTouched() {
    // retrieve the multiplayer context for the element
    var ctx = mp.context(el);

    // set the 'onPlay' bool to true, and in 30 seconds, set it back to false
    ctx.autoToggle('onPlay', true, 30000);

    // There is no need to set the prop value: autoToggle will take control of this
    // prop, setting it to true and false.
}
```

The most basic networking primitive provided is the auto-toggle. This construct sets a boolean prop to a specific value, and when a provided time elapses, resets the prop. The prop changes are propagated to all clients currently in the experience.

This construct is connection transparent-- meaning that a connection to the multiplayer server is not required. The prop will be set to a value and flipped back regardless of whether or not the client is connected. If the client is connected, the prop change will propagate to all other clients. A key point here is that the client is not responsible for setting the prop back to the original value when the time has elapsed: the multiplayer server will do this. This is important because if a client is disconnected or leaves the server, the timer will still elapse and updates will still be sent out to other clients.
