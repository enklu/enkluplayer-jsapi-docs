# Scene Editing

Enklu comes with a simple scene editing API. This allows scripts running in the scene to make changes to the scene. This should _not_ be used for multiplayer, but is a handy way to automate scene editing.

## Connection

```javascript
var edit = require('edit');

if (edit.isConnected) {
	start();
} else {
	edit.connect(function(err) {
		if (err) {
			log.error("Oh no! I couldn't connect : " + err);
		} else {
			start();
		}
	});
}
```

First, make sure the edit api is connected to the Enklu platform. This is generally done after checking the `isConnected` flag.

## Transactions

> First, create a transaction.

```javascript
var txn = edit.txns.create();
```

> Next, add actions to the transaction.

```javascript
txn
	.update(element.id, "bool", "visible", true)
	.update(element.id, "vec3", "position", vec3(0, 1, 0));
```

> Finally, make a request to apply the transaction.

```javascript
// fire and forget (useful for frequent updates)
edit.txns.request(txn);

// callback
edit.txns.requestCallback(txn, function(err) {
	if (err) {
		log.error('There was an error : ' + err);
	}
});
```

> You may also duplicate an element. This requires creating a unique id for the element ahead of time. This is so that the created element may be referenced with other actions within the same transaction.

```javascript
var id = edit.txns.generateId(); // generates a unique id for the new element
var txn = edit.txns
	.create()
	// include the element this new element should be a child of, the element to duplicate, and the id of the new element
	.duplicate(parentElement.id, templateElement.id, id)

	// now we can reference the created element before it's even created
	.update(id, "bool", "visible", false);
```

> Finally, delete elements with `delete`.

```javascript
var txn = edit.txns
	.create()
	.delete(id);
```

The scene can be edited through the use of transactions (txns). A transaction is a list of actions on a scene that are applied atomically. This means that if any of the actions fail, all of the actions are rolled back.