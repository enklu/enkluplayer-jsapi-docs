# Material

```javascript
if (!this.material) {
	log.error('Asset missing material support!');
	return;
}
```

The Material API is available on Elements and is used to modify material variables. Material variables are passed into shaders, to allow for dynamic effects/visuals. It's a good idea to null check first to ensure the target Element has support.

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
	var color = this.material.getColor('_Tint');
	this.material.setColor('_Tint', col(color.g, color.b, color.r, color.a));
}

```

Supported parameter types are: Float/Integer/Vec3/Col. All parameter types can be both retrieved and set.

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