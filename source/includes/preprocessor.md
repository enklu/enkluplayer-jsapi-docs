# Preprocessors

A powerful preprocessor is included as part of the `Behavior` and `VineML` execution pipeline. The preprocessor can process two types of blocks: schema and js blocks. The former allows injecting schema data, while the latter allows generating `VineML` or JavaScript via... JavaScript.

## Schema Preprocessor

> Values can be pulled from an element's schema.

```html
<Button label='{[label]}' />
```

> This will expose a 'label' string field in the editors and store the 'label' data in a prop on the element. A type may be specified as well.

```html
<Button label='{[amount:float]}' />
```

> This will inform the editor when building the editor field. Finally, you may also specify a default value. If a default value is specified, the type is required.

```html
<Button label='{[label:String = "Push Me!"]}' />
```

It is very useful to insert schema data into a `VineML` document or a `Behavior` script. This allows for scripts to be reused with different data on different elements. While `{{ }}` allow you to include JS, `{[ ]}` blocks allow you to include `Prop Definitions`. These definitions define the type of data needed, which expose inspector fields in Enklu editors.

## JS Preprocessor

> The value returned from the JS block is inserted into the `VineML` document.

```html
<?Vine>

<Menu>
	{{
		var elements = [];
		for (var i = 0; i < 100; i++) {
			elements.push("<Button label='" + i + "' />");
		}

		return elements.join('');
	}}
</Menu>
```

Sometimes it's useful to insert logic into a `VineML` document. In these cases, scripts can be embedded in a `VineML` document that will be executed by a JavaScript preprocessor before being handed off to the `VineML` parser.

## Advanced Usage

> We are generating `VineML` with JS by using the schema prop, `numElements`. The schema value is injected before the JS executes.

```javascript
<Menu>
	{{
		var result = [];
		for (var i = 0; i < {[numElements:int = 10]}; i++) {
			result.push("<Button label='" + i + "' />");
		}

		return result.join('');
	}}
</Menu>
```

Both the schema and the JS preprocessors are applied to every `VineML` document and `Behavior` script. They are always executed schema first, then JS. Because of this, one may be used inside another.