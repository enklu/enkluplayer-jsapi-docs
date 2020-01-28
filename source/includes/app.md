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

// focus on an element
app.sai.focus(button);
app.sai.focut(button, onComplete);

// follow the player
app.sai.unfocus();

// display some text
var message = "Hello, world.";
app.sai.write(message);

// speak in plain text or SSML
app.sai.speak(message, onComplete);

// teleport directly to an element
app.sai.teleport(button);

// or back to the player
app.sai.teleport();
```

The `sai` object can be used to control the Spatial Artificial Intelligence familiar. Use SAI to communicate essential information about a scene or direct a player's attention toward important elements.
