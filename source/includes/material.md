# Material

```javascript
if (!this.material) {
	log.error('Asset missing material support!');
	return;
}
```

The Material API is available on Elements and is used to modify material variables. Material variables are passed into shaders, to allow for dynamic effects/visuals. It's a good idea to null check first to ensure the target Element has support.

```javascript
this.material.makeUnique();
```

If you have multiple instances of the same asset in an Experience but want to control only the material of one instance, you can individually make materials unique. 
Note, this will incur some performance overhead but in most cases is negligible.

## Modifying Parameters

```javascript
// Float
function flipAlpha() {
	var alpha = this.material.getFloat('_FinalAlpha');
	this.material.setFloat('_FinalAlpha', 1 - alpha);
}

// Integer
function increaseTileCount() {
	var tiles = this.material.getInt('_TileCount');
	this.material.setInt('_TileCount', tiles + 1);
}

// Vec3
function inverseDirection() {
	var direction = this.material.getVector('_WindDir');
	this.material.setVector('_WindDir', v.scale(direction, -1));
}

// Col
function cycleColor() {
	var color = this.material.getColor('_Color');
	this.material.setColor('_Color', col(color.g, color.b, color.r, color.a));
}
```

Supported parameter types are: Float/Integer/Vec3/Col. All parameter types can be both retrieved and set.

### RenderQueue

> Set's the RenderQueue to be just after the default transparent pass.

```javascript
this.material.renderQueue = 3001;
```

It's possible to change the default RenderQueue of a material if needed. Changing between Opaque and Transparent queues can have unintended side effects depending on the Asset's shader.

## Tween Integration

```javascript
const Tween = requie('tween')

function fadeIn() {
	this.material.setFloat('_Alpha', 0);
	var twnAlpha = tween.number(this, 'material._Alpha')
		.to(1)
		.duration(2);
	twnAlpha.start();
}
```

Material parameters can be tweened via the Tweening API. Currnetly, parameters have to be set before used for the internal integration to link them.
