# Timers

```javascript
var timers = require('timers');
```

The Timers API allows functionality to be invoked after a specific amount of time has elapsed.


## setTimeout

```javascript
timers.setTimeout(function() {
  log.info('500ms has elapsed!');
}, 500);

// 500ms has elapsed!
```

Functions can be scheduled to be invoked after a specific amount of time has elapsed. Delays are measured in milliseconds. A unique id is returned from every `setTimeout` call.

## clearTimeout
```javascript
var id = timers.setTimeout(function(){
  log.info('500ms has elapsed!');
}, 500);

timers.clearTimeout(id);  // This cancels the waiting invocation.
```

The ids returned from calls to `setTimeout` can be used to cancel an awaiting timeout.
