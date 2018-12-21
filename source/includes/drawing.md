# Drawing

The drawing API is used to draw primitives. It can be used for debugging or as part of an experience.

## Registration and Filtering

```javascript
var drawing = require('drawing');

// this is part of my experience
drawing.register('ex', function(ctx) {
    //
});

// this is another part of my experience
drawing.register('ex.touch', function(ctx) {
    //
});

// this is for debugging!
drawing.register('debugging', function(ctx) {
   	// 
});
```

> The `filter` method will include every drawing method registered that *starts with* the specified string.

```javascript
debug.filter(''); // render everything
debug.filter('ex'); // render all functions that start with "ex"
debug.filter('ex.touch'); // render all functions that start with "ex.touch"
```

To start rendering with the drawing API, register a function. This function will be called every frame until unregistered. This function is registered with a category that defines when the function will be used. The category can be used later to filter out different drawing methods.

## Context

```javascript
drawing.register('ex', function(ctx) {
    ctx
    	.stroke(0, 0, 1)
    	.line(vec3(0, 0, 0), vec3(1, 0, 0));
});
```

Each registered function will be called with a drawing context. This context object has many useful properties for debugging or creative coding. Each function on the context returns the context so that calls may be chained together.

## Matrices

> Make the existing matrix an identity matrix.

```javascript
resetMatrix()
```

> Push a new identity matrix onto the stack.

```javascript
pushMatrix()
```

> Get rid of the current matrix and pop the previous one off the stack.

```javascript
popMatrix()
```

Before drawing, it's important to understand the matrix stack. This stack is used to transform primitives that are drawn. The current matrix transformation with be applied to any subsequent drawing calls.

Each time a registered callback is called, the stack will be cleared and the current matrix will be reset.

## Building a Matrix

```javascript
ctx
	.rotate(Math.PI, 0, 0)
	.translate(0, 0, 1)
	.translate(0, 1, 0)
	.scale(1, 2, 2);
```

Building a matrix sounds hard but it's been made quite simple by the translation, rotation, and scale methods. These methods immediately transform the current matrix.

## Drawing Properties

```javascript
stroke(col4(1, 0, 0, 1))
stroke(r, g, b)
stroke(r, g, b, a)
```

Currently the only supported property is line color. The `stroke` methods set the color that all primitives will be drawn with.

## Lines

> The `line` functions draw a line from a start point to end point.

```javascript
line(vec3(0, 0, 1), vec3(0, 0, 0))
line(0, 0, 1, 0, 0, 0)
```

> `lines` draws an array of line segments. These line segments must be specified with beginning and end.

```javascript
lines([
    vec3(0, 0, 0), vec3(0, 0, 1),
    vec3(0, 0, 0), vec3(0, 1, 0),
    vec3(0, 0, 0), vec3(1, 0, 0)
])
```

> `linestrip` is useful for chains rather than segments. Instead of each line segment needing two points, each point draws a line to the next.

```javascript
linestrip([
    vec3(0, 0, 0), vec3(1, 0, 1), vec3(0, 1, 0)
])
```

Lines may be drawn using one of the three types of `line` methods.

## 3D Primitives

> Draw a wire box.

```javascript
var width = 10,
    height = 7,
    depth = 5,
    size = 20;
ctx.box(width, height, depth);
ctx.box(size);
```

> Draw a unit sphere with configurable smoothness. Use matrices to scale. The smoothness parameter passed into `sphere` uses a subdivision method to construct the sphere geometry. In general a value higher than 2 or 3 will never be necessary as the number of vertices doubles each time.

```javascript
// default quality
ctx.sphere();

// higher quality
ctx.sphere(1);
ctx.sphere(2);
ctx.sphere(3);
```

> Draw an octohedron.

```javascript
ctx.octohedron();
```

> Draw a dodecahedron.

```javascript
ctx.dodecahedron();
```

Several types of 3D primitives are supported as well.

## Triangles

```javascript
var verts = [];
var triangles = [];

// construct the geo in some way
constructGeo(verts, triangles);

ctx.triangles(verts, triangles);
```

When drawing custom 3D shapes, it's best to use the `triangles` API rather than iterating and drawing triangles in JavaScript.