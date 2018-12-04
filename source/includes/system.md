# System

A global `system` object is provided to scripts. This object describes an API for working with objects outside of a specific experience-- like devices or experience management.

## Methods

```javascript
// Places the origin of the experience at the current device position.
system.recenter();

// Terminates the application. This function does nothing on non-HoloLens targets.
system.terminate();

// Restarts the application. This function does nothing on non-HoloLens targets.
system.restart();
```

A handful of methods are exposed for managing the system.

## Device

```javascript
// The running version of Enklu.
system.device.enkluVersion

// The version of UWP. This function returns an empty string on non-UWP platforms.
system.device.uwpVersion

// The battery percentage, between 0 and 1.
system.device.battery
```

Device specific information is available through the `device` property.

## Experiences

```javascript
system.experiences.refresh(function(error) {
	if (error) {
		log.error('There was an error!');

		// handle error
	}
});
```

The `experience` object allows a user to query and load their experiences.

Before using, it's best to call first call `refresh()`. This will check the network for updates and download any new experience data. Without calling this first, the experience API will be of limited use. Once `refresh` has been called, all of the other methods are synchronous.

```javascript
// get all of my experiences
var experiences = system.experiences.all();

// get only specific experiences
var a = system.experiences.byName('Zoo');
var b = system.experiences.byId(id);
```

> Each experience data object that comes back will have the following properties.

```javascript
{
	id,				// string - unique id of this experience
	name,			// string
	description,	// string
	isPublic,		// boolean - true iff the experience is shared with the public
	createdAt,		// string - UTC timestamp
	updatedAt		// string - UTC timestamp
}
```

> Finally experiences may be played. Note that these methods require the unique id of the experience.

```javascript
// enter play mode
system.experiences.play(a.id);

// enter edit mode
system.experiences.edit(a.id);
```

Experiences may be retrieved in a variety of ways.