# Audio

```javascript
if (!this.audio) {
	log.error('Asset missing audio support!');
	return;
}
```

The Audio API is available on any Element whose Asset contains an audio clip. It's recommended to use a null check to ensure all instances of your script have their dependencies.

## Modifying Properties

```javascript
// Invert looping
this.audio.loop = !this.audio.loop;

// Mute
this.audio.mute = true;

// Volume
this.audio.volume = 0.5;
```

Each of the following properties can be retrieved and set:

- volume
- loop
- mute
- playOnAwake
- spatialBlend
- minDistance
- maxDistance
- dopplerLevel

## Tween Integration

```javascript
const tween = require('tween');

var twnVolume = tween.number(this, 'audio.volume')
	.from(0)
	.to(1)
	.duration(10);
twnVolume.start();
```

Tweening can be used to modify volume over time.