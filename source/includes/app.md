# App

In all scripts, there is a global `app` object. This object is useful for manipulating application-wide settings, scenes inside an experience, creating new elements via scripts, and networking.

## Scene Object

> Retrieves ids of all scenes

```javascript
var all = app.scenes.all;
```
> Returns root element of a specific scene

```javascript
var root = app.scenes.root('myScene');
```

The `scenes` object has functions for manipulating multi-scene experiences.

## Element Object

> Retrieves element

```javascript
button = app.elements.byId('specific-id');
```

> Creates element as a child of another

```javascript
var a = element.create(root, 'Button');
var b = element.create(a, 'Button', 'specific-id');
```

> Creates elements from a vine, as a child of `a`

```javascript
var c = element.createFromVine(a, '<Menu><Button /></Menu>');
```

> Destroys an element

```javascript
app.elements.destroy(button);
```

The `elements` object can be used to query all scenes for an element, create elements, or destroy them.

## SAI object

> Enables SAI

```javascript
app.sai.enabled = true;
```

> Focuses SAI on an element or the player

```javascript
app.sai.focus(button);
app.sai.focus(button, onComplete);
app.sai.unfocus();
```

> Displays some text next to SAI

```javascript
var message = "Hello, world.";
app.sai.write(message);
```

> Speaks in plain text or SSML

```javascript
app.sai.speak(message);
app.sai.speak(message, onComplete);
```

> Teleports SAI directly to an element or back to the player

```javascript
app.sai.teleport(button);
app.sai.teleport();
```

The `sai` object can be used to control the Spatial Artificial Intelligence familiar. Use SAI to communicate essential information about a scene or direct a player's attention toward an important element.
