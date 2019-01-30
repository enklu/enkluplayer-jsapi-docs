# Voice

The Voice API provides access to registering callbacks when certain keywords are recognized.

```javascript
const voice = require('voice');
```

## Registration

A keyword can be registered with `register` or `registerAdmin`. Admin keywords require the word "admin" to be spoken before being recognized.
A `boolean` is returned showing the registration's success.

```javascript
function enter() {
  var explodeSuccess = voice.register('explode', function() {
    myExplosionAsset.visible = true;
  });
  log.info('Explode added: ' + explodeSuccess);
  
  var cleanupSuccess = voice.registerAdmin('cleanup', function() {
    myExplosionAsset.visible = false;
  });
  log.info('Cleanup added: ' + cleanupSuccess);
}

function exit() {
  voice.unregister('explode');
  voice.unregister('cleanup');
}
```
