# Random

The Random API provides several utilities for random value generation that aren't covered by the JS `Math` API.

## Floats

> First, include the require.

```javascript
const random = require('random-preview');
```

> Generate a random float in (0, 1).

```javascript
var val = random.value();
```

> Generate a random float in [min, max].

```javascript
var val = random.range(0, 10);
```

> Generate a random int in [min, max]

```javascript
var val = random.rangeInt(0, 10);
```

> Generate a random index for an array. This will return an int value in [0, len).

```javascript
var val = random.index(myArray.length);
```

## 3D

> Generate a random Quat. This method will produce a distribution that is uniform across the full rotation of each axis, rather than component wise, which produces an unequally weighted distribution.

```javascript
var a = random.quat();
```

> Generate a random distribution of two dimensional vectors.

```javascript
var vecs = random.distribution(radius, count);
```

Use the `random-preview` import to access extended random value utilities. These values are **not** intended to be used in cryptography.

All documentation uses proper interval notation. For example, `[0, 1)` means the value may equal 0 but excludes 1.