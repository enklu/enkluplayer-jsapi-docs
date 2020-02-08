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

## SAI object (Preview)

> Enables SAI

```javascript
app.sai.enabled = true;
```

> Focuses SAI on an element. An optional callback can be provided that will be invoked when SAI reaches the target element.

```javascript
app.sai.focus(button);
app.sai.focus(button, onComplete);
```

> Removes an exising focus target and tells SAI to follow the player.

```javascript
app.sai.unfocus();
```

> Displays some text next to SAI. An optional timeout can be provided that will clear the text when it expires. If the timeout is set, a callback can also be provided that will be invoked when the timeout expires.

```javascript
var message = "Hello, world.";
app.sai.write(message);
app.sai.write(message, timeoutMs);
app.sai.wrtie(message, timeoutMs, onComplete);
```

> Speaks in plain text or SSML. An optional callback can be provided that will be invoked after speech synthesis playback completes.

```javascript
app.sai.speak(message);
app.sai.speak(message, onComplete);
```

> Teleports SAI directly to an element or back to the player

```javascript
app.sai.teleport(button);
app.sai.teleport();
```

> Plays one of SAI's animations Currently supported values are `Happy`, `Sad`, `Idle`, `Move`, `Pulse`, `Talk`, `Scan`, `Highlight`, and `Work`. 

```javascript
app.sai.animate('Happy');
```

The `sai` object can be used to control the Spatial Artificial Intelligence familiar. Use SAI to communicate essential information about a scene or direct a player's attention toward an important element.
