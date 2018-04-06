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
