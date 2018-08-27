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

## Networking (PREVIEW -- Not Yet Released)

```javascript
var element = app.elements.byId('myElement');
app.network.sync(
	element,
	function(el, evt) {
		// evt.type = 'create' | 'update' | 'move' | delete'

		if (evt.type === 'update') {
			// sync only position
		    if (evt.prop.name === 'position') {
		    	if (evt.prop.prev != evt.prop.next) {
		    		return true;
		    	}
		    }
		}
	    
	    return false;
	});

// ...

app.network.unsync(element);
```

The `network` object can synchronize changes across all users. This can be done via a single function: `sync`. Simply pass in an element, and define a callback. This callback will be called when any change occurs to this element _or any child_ of the element. The callback will be called with the `element` that has changed as well as the type of update: create, update, move, or delete. The event passed in includes details about what happened. Returning true from the callback means the change should _attempt_ to be synchronized, whereas false means it should not be.