# Math

The Math API extends the builtin [JS Math API](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math), which is already fully supported. Enklu includes objects for working in three dimensions.

## Vec3

> Create a new vec3 with the function vec3().

```javascript
var pos = vec3(0, 0, 0);    // { x:0, y:0, z:0 }
```

> The v object is provided with several constants.

```javascript
v.zero;     // { x:0, y:0, z:0 }
v.one;      // { x:1, y:1, z:1 }
v.up;       // { x:0, y:1, z:0 }
v.right;    // { x:1, y:0, z:0 }
v.forward;  // { x:0, y:0, z:1 }
```

> The vec3 is not immutable, but for vector math functions, vec3s are treated as immutable objects.

```javascript
var a = vec3.up;                // { x:0, y:1, z:0 }

// scalar multiplication
v.scale(a, 5);                   // { x:0, y:5, z:0 }

// vector addition
v.add(a, vec3(1, 0, 0));        // { x:1, y:1, z:0 }

// dot product
v.dot(a, vec3.forward);         // 0

// cross product
v.cross(a, vec3.right);         // { x:0, y:0, z:1 }

// vector length
v.len(a);                       // 1

// normalize
v.normalize(vec3(10, 0, 0));    // { x:1, y:0, z:0 }

// distance
v.distance(a, vec3(1, 0, 0));   // 1.4142

// horizontal distance
v.distanceXZ(a, vec3(1, 5, 0));   // 1.4142

// The length & distance functions come with squared versions for performance optimizations
v.lenSqr(vec3(0, 2, 0));            // 4
v.distanceSqr(a, vec3(1, 0, 0))     // 2
v.distanceXZSqr(a, vec3(1, 5, 0))   // 2

```

The `vec3` object is useful for working with `Element` translation and scale, or any other 3D vector math.

## Quat

> Not usually useful, but for completeness a quat constructor is provided which accepts hamiltonians.

```javascript
var rot = quat(1, 0, 0, 1);
```

> More usefully, the q object is provided for constants and basic operations

```javascript
q.identity;         // identity rotation

q.eul(90, 0, 0);    // creates a quat from Euler rotations
q.euler(90, 0, 0);  // identical to q.eul
```

The `quat` is provided for three dimensional rotation.
