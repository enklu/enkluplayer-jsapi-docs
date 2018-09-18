# Messages

> ElementA

```javascript
var messages = require('messages');

var treasure = this.find('..(@name=Treasure)');

function showTreasure(visible) {
  treasure.visible = visible;
}

messages.on('show-treasure', showTreasure);
```

> ElementB

```javascript
var messages = require('messages');

messages.dispatch('show-treasure', true);
```

The Messages API provides global message dispatching for your experience's scripting. Messages can be dispatched via `dispatch` and listened to with `on` and `off`.
