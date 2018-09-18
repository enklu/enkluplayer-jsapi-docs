# Logging

> Use the log object.

```javascript
log.debug('Hello World!');
```

> The log API supports four levels.

``` javascript
log.debug('Debug level.');
log.info('Info level.');
log.warn('This is a warning!');
log.error('Something bad happened!');
```

> Each logging method also supports C# style string replacement.

```javascript
log.debug('This is a {0} log.', 'debug');
```

The logging API is useful for debugging, and is only active for app collaborators in edit mode. Once an app is deployed or viewed in play mode, logging will no longer be visible.

All logs are output to the browser console when working from the web editor. Logging support on other platforms is coming soon.
