# VineML

`VineML` is a powerful markup language designed to be very familiar to anyone that has used HTML. It is useful for creating any type of element hierarchy in a scene and assigning values to those elements. A combination of `VineML` and `Behavior` scripts can be used to create rich applications in Enklu.

## Language QuickStart

```html
/* A vine header is optional.*/
<?Vine>

/* Documents can have only a single root element. */
<Float>
	/* Tags can be self closing like this button. */
	<Button label='Hello World!' />
</Float>
```

> Note that `VineML` supports C-style block comments.

`VineML` files are called _documents_. These documents are made up of a hierarchy of nested `tags`, starting from a single root element. Each tag can have zero or more attributes. To create your first `VineML` document, it's easiest to jump right in with an example.

## Tags

> This is not valid `VineML`.

```html
<Container>
```

> These are both valid options.

```html
<Container></Container>

<Container />
```

> This is not valid `VineML`.

```html
<Caption>Hello World</Caption>
```

> Instead we use attributes.

```html
<Caption label='Hello World' />
```

Tags follow XHTML conventions, i.e. every tag must be closed. In HTML, we can write `<br>`, but in `VineML` this is invalid. Every tag must have a closing tag or it must be a self closing tag.

In addition, tags may not have raw text inside of them. All values are passed to tags through attributes.

## Attributes

```html
<Caption
	label='Hello'
	visible=false 
	font.size=100
	width=1000.5
	position=(0, 0, 10)
/>
```

While tags define the object structure, attributes define the object properties. `VineML` has support for string, boolean, number, and vector literals. Any property of an element may be passed through element attributes.