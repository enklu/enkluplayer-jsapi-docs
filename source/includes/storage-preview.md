# Storage (Preview)

```javascript
const storage = require('storage-preview').local;
```

The Storage API allows an experience to cache persistent data across users, per device. The interface behaves similarly to the current web standards. 

Note: Any data saved in the web editor will be removed when the session ends.

## Accessing Data

> Save data to storage.

```javascript
var data = {
  points: 26,
  currency: 42
}
storage.setItem('session', JSON.stringify(data));
```

> Get data from storage.

```javascript
var data = JSON.parse(storage.getItem('session'));
if (data) {
  log.info('Previous data found.');
  log.info('  Points: ' + data.points);
  log.info('  Currency: ' + data.currency);
}
```

Data can be saved and retrieved through `setItem` and `getItem`. Complex objects can be used with `JSON.stringify` and `JSON.parse`.

## Removing Data

> Remove a single item.

```javascript
storage.removeItem('session');
```

> Remove all items.

```javascript
storage.clear();
```

Data can be removed from storage via `removeItem` and `clear`. Clear will only remove data for the currently loaded experience.

## Keys

> Get the total number of keys.

```javascript
log.info('Storage contains: ' + storage.length + ' entries.');
```

> Get the key for a specific index.

```javascript
log.info('Key: ' + storage.key(0));
```

> Iterate over storage data.

```javascript
const numKeys = storage.length;
for (var i = 0; i < numKeys; i++) {
  const key = storage.key(i);
  const data = JSON.parse(storage.getItem(key));
  log.info(key + ': ' + data);
}
```

Information about keys can be accessed via `length` and `key`. This can be useful to iterate over storage data.
