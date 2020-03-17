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

## setInterval

```javascript
timers.setInterval(function() {
  log.info('The time is: ' + time.now());
}, 1000);
```

Functions can be scheduled to be invoked on a specified interval. Delays are measured in milliseconds, and will continue to keep invoking repeatedly with the same interval until canceled. A unique id is returned from every `setInterval` call, to be used with `clearInterval`.

## clearInterval

```javascript
var id = timers.setInterval(function() {
  log.info('The time is: ' + time.now());
}, 1000);

timers.clearInterval(id);
```

The ids returned from calls to `setInterval` can be used to cancel an interval. It is recommended to always ensure `clearInterval` is called in a script's `exit` function if not cleared sooner.
