# Checksum

The `checksum` library provides limited apis related to insecure checksum algorithms. These algorithms are NOT cryptographically secure and should not be used for secure information.

## CRC

```javascript
var hash = require('checksum').crc16('Whan that Aprill with his shoures soote');
```

Use the `crc16` method to retrieve a deterministic 16-bit integer from a string.