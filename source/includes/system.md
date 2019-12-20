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
    id,             // string - unique id of this experience
    name,           // string
    description,    // string
    isPublic,       // boolean - true iff the experience is shared with the public
    createdAt,      // string - UTC timestamp
    updatedAt       // string - UTC timestamp
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

## Login

> Check if the user is logged in.

```javascript
log.info(system.isLoggedIn);
```

> Prompt the user to Login.

```javascript
system.login();
```

> Logs out the current user.

```javascript
system.logout();
```

Scripts can check if a user is logged into an account or using a guest account to view a public or bundled experience. The user can be prompted to login, or logged out through this API as well.

## Session

> Start a new session

```javascript
system.session.start(function(error, sessionInfo) {
    if (error) {
        log.warn('Could not start session: ' + error);
        return;
    } 

    log.info('Started a new session for user: ' + sessionInfo.user.id);
});
```

> Stop a current session

```javascript
system.session.stop();
```

> Access to the current session information

```javascript
var sessionInfo = system.session.current;
var sessionId = sessionInfo.sessionId;
var user = sessionInfo.user;
var userId = user.id;
var inventory = user.items;
```

> Listen for sessions to start and end from another script

```javascript
system.session.on('created', function(sessionInfo) {
    log.info('Started a new session for user: ' + sessionInfo.user.id);
});

system.session.on('ended', function() {
    log.info('Session ended');
});
```

A session is a timespan (set by `start` and `stop` calls) that associates specific player actions with a user. Calling `start` on the session api will bring up a QR code scanner which will prompt a user to scan the QR code generated on their mobile device by the Enklu app. Once the QR is scanned and the session is created, it is possible for any content generation or player action to incorporate session data. For example, snaps which are created and uploaded during an active session will generate an association between the session user and the specific snap taken. Any snaps associated with a session will show up in the user's "My Feed" section of the Enklu mobile app.


## User

A user is currently bound to each session. While a session represents a specific timeframe within an experience, a user provides a persistent collection of data. Currently, a user consists of `id` and `items`.

> Check to see if a user has a specific item

```javascript
system.session.on('created', function(sessionInfo) {
    var user = sessionInfo.user;
    var items = user.items;

    var foundItem = false;
    for (var i = 0; i < items.length; ++i) {
        if (items[i].name === 'Light Saber') {
            foundItem = true;
            break;
        }
    }

    log.info('Found Light Saber: ' + foundItem);
});
```

## Inventory

A user may own specific items which will populate on the `user` object within a `session`. 

> Item Properties

```javascript
{
    // The inventory item's identifier.
    id: <guid>,

    // The name of the inventory item. 
    name: <string>,

    // A description of the item.
    description: <string>,

    // The name of the thumbnail as it appears 
    // in the environments inventory bucket.
    thumbnail: <uri>

    // The total quantity of the item the user owns.
    quantity: <number>
}
```

## Cursor

> Forces the cursor to be shown.

```javascript
system.cursor.forceShow = true;
```

By default, the Cursor is only visible when it is near an IUX activator. Setting `forceShow` to be `true` enables the cursor to always be shown.


## Camera

> Changes the near & far planes, measured in meters.

```javascript
system.camera.nearPlane = 0.75;
system.camera.farPlane = 20;
```

The camera's near and far planes can be modified through scripts to modify the default frustum culling ranges.
