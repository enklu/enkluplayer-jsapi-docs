# Tween

```javascript
var tw = require('tween');
```

The Tween API provides element tweening operations. These are **highly recommended** over tweening objects "by hand" inside of your own update loops. Not only does the tween API provide excellent easing functions and a terse API, it is implemented as part of the player runtime so it is highly optimized.


## Creating a Tween

```javascript
// get the element
var el = this.findOne('..tweener');

// create a tween on the vec3 'scale' prop
var vectorTween = tw.vec3(el, 'scale');

// create a tween on the col4 'color' prop
var colorTween = tw.col4(el, 'color');

// create a tween on the number 'alpha' prop
var alphaTween = tw.number(el, 'alpha');
```

> Neither the name nor the type of the prop may be changed after the tween has been created.

Tweens are created specific to the `name` and `type` of prop they affect. To that end, we provide three types of tweens: `number`, `vec3`, and `col4`. The vector types are tweened component-wise.

## Configuring a tween

```javascript
var a = tw
	.number(this.findOne('..covering'), 'alpha')

	// (required) The value to tween to.
	.to(1)

	// (optional) The starting value. If not specified, the starting value will be the current value.
	.from(0)

	// (optional) The duration, in seconds, of the tween. Defaults to 1.
	.duration(2.5)

	// (optional) The delay, in seconds, before the tween should start.
	.delay(1)

	// (optional) The easing equation to use. Defaults to tw.easing.Linear.
	.easing(tw.easing.CubicOut)

	// (optional) A function to call when the tween is first started.
	.onStart(function() { log.info('Started!'); })

	// (optional) A function to call after the tween has been completed
	.onComplete(function() { log.info('Complete!'); });
```

Each parameter on a tween is configured through a corresponding function on the tween. These functions can be chained to configure tweens very quickly and easily.

Most parameters are _optional_. The only required parameter is `to`, which provides the value to tween to.

## Tween Playback

```javascript
var a = tw.number(el, 'foo').to(1);

// starts the tween
a.start();

// pauses the tween at the current time
a.pause();

// resumes the tween from the paused state
a.resume();

// stops the tween
a.stop();
```

> Note that none of these functions return the tween object. This is intentional as nothing may be configured after `start` has been called.

Once a tween is created, playback is controlled by functions on the tween itself. Note that once a tween has been started, the tween's configuration may not be changed.