# Elements

## Overview

The Elements API is powerful abstraction that allows users to manipulate any property on any Element through a handful of simple methods. This is important because these few methods allow for easy networking or persistent state.

## Element Hierarchy

> An element's children are accessible via the readonly children field.

```javascript
var children = element.children;
for (var i = 0, len = children.length; i++) {
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

An Element may have zero to many children, whicn are simply other Elements, themselves having children. This forms a sort of directed graph, visualized by the tree component in the web editor.

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
        'Value changed from {0} -> {1}.",
        prev,
        next);
});

element.setNumber('foo', 5);  // Value changed from -7 -> 5.
```

> If an element doesn't already have a value, it returns a value up the graph. In this circumstance, the property of b is _bound_ to the property of a.

```javascript
a.addChild(b);

a.setNumber('foo', 12);
b.getNumber('foo');         // 12

a.setNumber('foo', 17);
b.getNumber('foo');         // 17
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
element.schema.setVec('position', vec3(1, 1, 1));
element.transform.position = vec3(1, 1, 1);

// these two are equivalent
element.schema.setVec('scale', vec3(1, 1, 1));
element.transform.scale = vec3(1, 1, 1);

// these two are equivalent
element.schema.setQuat('rotation', q.euler(0, 0, 0));
element.transform.rotation = q.euler(0, 0, 0);
```

Several shortcuts are provided, not least of which is the `transform` object. While `localPosition`, `localRotation`, and `localScale` may all be set via the Schema system, the `transform` object also provides these as raw fields.
