# App

In all scripts, there is a global `app` object. This object is useful for manipulating application-wide settings, scenes inside an experience, creating new elements via scripts, and networking.

## Scene Object

```javascript
// retrieves ids of all scenes
var all = app.scenes.all;

// returns root element of a specific scene
var root = app.scenes.root('myScene');
```

The `scenes` object has functions for manipulating multi-scene experiences.

## Element Object

```javascript
// retrieves element
button = app.elements.byId('specific-id');

// creates element as a child of another
var a = element.create(root, 'Button');
var b = element.create(a, 'Button', 'specific-id');

// creates elements from a vine, as a child of a
var c = element.createFromVine(a, '<Menu><Button /></Menu>');

// destroys element
app.elements.destroy(button);
```

The `elements` object can be used to query all scenes for an element, create elements, or destroy them.

## SAI object

```javascript
// enables SAI
app.sai.enabled = true;

// focuses SAI on an element or the player
app.sai.focus(button);
app.sai.focus(button, onComplete);
app.sai.unfocus();

// displays some text next to SAI
var message = "Hello, world.";
app.sai.write(message);

// speaks in plain text or SSML
app.sai.speak(message);
app.sai.speak(message, onComplete);

// teleports SAI directly to an element or back to the player
app.sai.teleport(button);
app.sai.teleport();
```

The `sai` object can be used to control the Spatial Artificial Intelligence familiar. Use SAI to communicate essential information about a scene or direct a player's attention toward important elements.
