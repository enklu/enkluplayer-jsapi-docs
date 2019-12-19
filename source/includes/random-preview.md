# Random (Preview)

```javascript
const random = require('random-preview');
```

An API full of helpful functions for generating random values from a variety of ranges.

## Values

```javascript
// Returns a random value between 0 and 1, exclusively.
log.info(random.value());

// Returns a random number between min and max, inclusively.
log.info(random.range(2, 8))

// Returns a random int between min and max, inclusively.
log.info(random.rangeInt(2, 8))

// Returns a random index for a collection's length.
var tmp = [ 1, 2, 3 ];
log.info(random.index(tmp.length));

// Returns a random distribution, controlled by size (inclusive) and quantity/
var dist = random.distribution(3, 5);
for (const entry of dist) {
  log.info(entry.x, entry.y);
}
```

Random values and ranges can be generated with methods like `.value()` and `.range(<min>, <max>)`.

## Objects

```javascript
this.transform.rotation = random.quat();
```

A random Quaternion can be generated as well.
