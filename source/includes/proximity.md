# Proximity

```javascript
var proximity = require('proximity');
```

The Proximity API allows functions in `Behavior` scripts to be executed as a response to proximity events on Elements. This API is available via `require` statements. Proximity events are dispatched when a Trigger enters a subscribed Element's radius. By default the user is a Trigger, and other Elements can be set as Triggers through their schema. Additionally, a trigger or subscribed Element's radius can be configured through schema editing.

## Events

- `enter` - Dispatched when a Trigger first enters a subscribed Element's `innerRadius`..
- `stay` - Dispatched every frame after a Trigger enters a subscribed Element's `innerRadius`, but hasn't exited its `outerRadius`.
- `exit` - Dispatched when a Trigger exits a subscribed Element's `outerRadius`.

## Subscriptions

```javascript
function enter(){
	proximity.subscribe(this, 'enter', onTriggerEnter);
	proximity.subscribe(this, 'exit', onTriggerExit);
}

function onTriggerEnter(){
	this.transform.scale = vec3(0.5, 0.5, 0.5);
}

function onTriggerExit(){
	this.transform.scale = vec3(1, 1, 1);
}

function exit(){
	proximity.unsubscribe(this, 'enter', onTriggerEnter);
	proximity.unsubscribe(this, 'exit', onTriggerExit);	
}
```

Any Element can be subscribed for a proximity event. Callbacks are invoked with a reference to the Trigger that entered its `innerRadius`. Any Trigger that is above or below a subscribed Element in the hierarchy won't invoke events. It is good practice to call subscribe in a script's `enter` and unsubscribe in `exit`.

## Schema

```javascript
this.schema.setNumber('proximity.trigger', true);
this.schema.setNumber('proximity.innerRadius', 0.25);
this.schema.setNumber('proximity.outerRadius', 2);
```

All schema configuration is done under the `proximity` namespace:
- `proximity.trigger` - Marks an Element as a Trigger.
- `proximity.innerRadius` - The distance where a subscribed Element or Trigger will dispatch `enter` events.
- `proximity.outerRadius` - The distance a Trigger or subscribed Element has to be to dispatch `exit` events.