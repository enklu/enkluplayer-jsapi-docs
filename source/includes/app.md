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
