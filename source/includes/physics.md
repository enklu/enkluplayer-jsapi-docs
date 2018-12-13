# Physics

The Physics API provides access to simple raycasting. In time, they'll be more functionality available.

```javascript
const physics = require('physics');
```

## Raycasting

A raycast can be used to see if a ray intersects with a specific Element.

```javascript
var element = this.findOne('..(@name=Cube)');

var hit = physics.raycast(v.zero, v.forward, element);

if (hit) {
	log.info('Hit! ' + hit);
} else {
	log.info('Miss :(');
}
```