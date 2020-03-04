# Elements

The Elements API is a powerful abstraction that allows users to manipulate any property on any Element through a handful of simple methods. This is important because these few methods allow for easy networking or persistent state.

## Hierarchy

> An element's children are accessible via the readonly children field.

```javascript
var children = element.children;
for (var i = 0, len = children.length; i < len; i++) {
    var child = children[i];
}
```

> Child elements may be added and removed.

```javascript
a.children;         // []
a.addChild(b);      // [b]
a.removeChild(b);   // []
```

> Adding an element as a child to one element will remove it from another.

```javascript
a.addChild(b);      // a.children = [b]
c.addChild(b);      // a.children = [], c.children = [b]
```

> Adding an element to its parent will reorder the children.

```javascript
a.addChild(b);      // [a]
a.addChild(c);      // [a, c]
a.addChild(a);      // [c, a]
```

> Relationships can be tested explicitly or with helpers.

```javascript
b.parent === a;   // True
b.isChildOf(a);   // True
```

An Element may have zero to many children, which are simply other Elements, themselves having children. This forms a sort of directed graph, visualized by the tree component in the web editor.

## Destroy

```javascript
// destroys element and all children
element.destroy();
```

## Schema

> Use get and set methods to retrieve properties for primitive types.

```javascript
// strings
element.schema.setString('foo', 'This is a foo.');
element.schema.getString('foo');                    // This is a foo.

// numbers
element.schema.setNumber('bar', 12);
element.schema.getNumber('bar');                    // 12

// bools
element.schema.setBool('fizz', true);
element.schema.getBool('fizz');                     // true
```

> Properties may also be watched for changes.

```javascript
element.setNumber('foo', -1);
element.watchNumber('foo', function (prev, next) {
    log.info(
        'Value changed from {0} -> {1}.',
        prev,
        next);
});

element.setNumber('foo', 5);  // Value changed from -1 -> 5.
```

> Watched properties can be unwatched.

```javascript
function onChange(prev, next){
  log.info('Value changes from {0} -> {1}.',
    prev,
    next);
}

element.setNumber('foo', -1);
element.watchNumber('foo', onChange);
element.setNumber('foo', 2);            // Value changes from -1 -> 2

element.unwatchNumber('foo', onChange);
element.setNumber('foo', 2);            // <no output>
```

> Properties can be watched explicitly for one change

```javascript
element.setNumber('foo', -1);
element.watchNumber('foo', function (prev, next) {
    log.info(
        'Value changed from {0} -> {1}.',
        prev,
        next);
});

element.setNumber('foo', 5);  // Value changed from -1 -> 5.
element.setNumber('foo', 8);  // <no output>
```

> If an element doesn't already have a value, it returns a value up the graph. In this circumstance, the property of b is _bound_ to the property of a.

```javascript
a.addChild(b);

a.setNumber('foo', 12);
b.getNumber('foo');         // 12

a.setNumber('foo', 17);
b.getNumber('foo');         // 17
```

> Values can be taken from an Element with explicitly avoiding searching up the graph. A default value for the data type will be set & returned if unprovided.

```javascript
b.getOwnNumber('foo');        // 0
b.getOwnNumber('foo', 12);    // 12

b.getOwnString('bar');        // <empty string>
b.getOwnString('bar', 'flip');// flip

b.getOwnBool('fizz');         // false
b.getOwnBool('fizz', true);   // true
```

> Watchers are also called for bound properties.

```javascript

b.watchNumber('foo', function(prev, next) {
    log.info('Changed!');
});
a.setNumber('foo', 12);     // Changed!

```

> Once the child changes its value, the binding is broken.

```javascript
b.setNumber('foo', -7);

a.getNumber('foo');         // 17
b.getNumber('foo');         // -7
```

Elements are designed to have a flexible method of managing, composing, and synchronizing state. To that end, each element has a `schema` object that stores all of the Element's state. Schema are a grab bag of values, each of which may be watched for changes. Additionally, Schema are composable up the graph-- that is, if an element's schema does not contain a specific value, it will look up the graph until the value is found. This is best seen by example.

## Queries

> Element::findOne() retrieves a child with matching id.

```javascript
// a
//  -b
var b = a.findOne('b');
```

> You can search for immediate children with the dot (.) operator.

```javascript
// a
//  -b
//    -c
//  -d
//    -e
//      -f
var f = a.findOne('d.e.f');
```

> Sometimes you may not know intermediate children. For a recursive search, use the double dot (..) operator.

```javascript
var f = a.findOne('..f');
```

> These can be combined.

```javascript
// a
//  -b
//    ...
//       -z
var q = a.findOne('b..f..p.q');
```

> Any element property can be used to filter by using the @ operator.

```javascript
var big = a.findOne('..(@size==10)');
```

> Finally, a collection of matching objects can be retrieved by using find.

```javascript
var allBig = a.find('b..q.(@size==10)');
```

It can become taxing to iterate the element graph to find the elements you need, particularly as it grows dynamically. For this reason, elements have a builtin query language that can be used to filter and find collections of elements that you need.

## Transform

```javascript
// these two are equivalent
element.schema.setVec3('position', vec3(1, 1, 1));
element.transform.position = vec3(1, 1, 1);

// these two are equivalent
element.schema.setVec3('scale', vec3(1, 1, 1));
element.transform.scale = vec3(1, 1, 1);

// these two are equivalent
element.schema.setVec3('rotation', vec3(0, 0, 0)); // Internally stored as Euler values.
element.transform.rotation = q.euler(0, 0, 0);
```

Several shortcuts are provided, not least of which is the `transform` object. While `localPosition`, `localRotation`, and `localScale` may all be set via the Schema system, the `transform` object also provides these as raw fields.


> World space values exist, though they should be used with care. On HoloLens, world space values include any transform changes an Anchor uses to locate its place in a space.

```javascript
element.transform.worldPosition;
element.transform.worldRotation;
element.transform.worldScale;
```

> Helper functions exist to manage conversions between world & local coordinate systems.

```javascript
// Transforms a Vec3 from local space to world space
element.transform.localToWorld(vec3(0, 0, 1));

// Transforms a Vec3 from world space to local space
element.transform.worldToLocal(vec3(10, 3, 5));
```

> A transform's local forward direction can be used.

```javascript
element.transform.forward;
```

> Miscellaneous

```javascript
// Turns an Element to look in a given direction.
element.transform.lookAt(v.up);
```

## Events

```javascript
a.on('activated', function(evt) {
	//
});

a.off('activated', onActivated);
```

Many elements dispatch events. These events can be listened for using `on` and `off` functions, which attach a callback to a specific event. It is good practice to attach events on `enter` and remove them in `exit`.

## Message Passing

```javascript
var menu = this.find('..menu-new');

// sends 'open' message to all attached scripts
menu.send('open');	// open()
menu.send('register', 'foo', 'bar', 5); // register('foo', 'bar', 5);
```

> If a `Behavior` script cannot handle a message the message is discarded. However, a special function may be defined which receives all discarded events.

```javascript
function msgMissing(type, args) {
	log.info(type + ' was called but I could not handle it.');
}
```

Messages may be passed to elements. The message consists of a function name and any number of parameters. The receiving element then calls the function by name on all attached scripts with the passed in parameters.

## Misc

```javascript
a.id;             // Readonly - Gets the Element's ID

a.type;           // Readonly = Gets the Element's type

a.visible = true; // Gets or sets this Element's visibility
```

The Element API has a few other miscellaneous usages for scripting.
