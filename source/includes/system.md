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

> The currently running version of Enklu. This value will vary between platforms, but is always in a semver form.

```javascript
var v = system.device.enkluVersion;
```

> The version string of UWP. This function returns an empty string on non-UWP platforms.

```javascript
var v = system.device.uwpVersion;
```

> The battery percentage given as a value between 0 and 1.

```javascript
var batt = system.device.battery;
```

> A unique id for this hardware. This value is constant between sessions and even application updates. A new install will change the value.

```javascript
var id = system.device.hardwareId;
```

Device specific information is available through the `device` property.

### Video Capture
```javascript
// Whether the device is currently recording.
log.info('Recording: ' + system.device.video.isRecording);

// Gets the device ready to record videos.
system.device.video.setup(function(success) {
  log.info('Video ready: ' + success);
});

// Starts recording a video.
system.device.video.start(function(success) {
  log.info('Recording started: ' + success);
});

// Starts recording a video to a custom path.
system.device.video.startCustomPath(function(success) {
  log.info('Recording started:' + success);
});

// Stops recording a video.
system.device.video.stop(function(path) {
  log.info('Video saved to: ' + path);
});

// Cleans up video capture on the device.
system.device.video.teardown();
```

Video Capture is available on devices that support it. Due to the asynchronous nature of underlying libraries, all scripting logic that depends on video capture status should occur in the callbacks provided. 
If you use video capture in your experience, it's best to call `system.device.video.teardown();` in an `exit()` function. 

### Image Capture
```javascript
// Optional - Potentially reduces `capture()` call time on certain devices.
system.device.image.warm(function(success) {
  log.info(success);
});

// Captures an image.
system.device.image.capture(function(path) {
  log.info('Image taken: ' + path);
});

// Captures an image with a specific path.
system.device.image.captureCustomPath(function(path) {
  log.info('Image taken: ' + path)
}, 'myImage');

system.device.image.abort(function(success) {
  log.info('Image capture aborted: ' + success);
});
```

Image capture is available on devices that support it. Note: This API is likely to change in the future to be more aligned with the video capture API.

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