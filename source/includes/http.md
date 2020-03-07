# HTTP (Preview)

> Use require to get access to the HTTP preview interface.

```javascript
const http = require('http-preview');
```

The HTTP API is currently in _preview_, meaning that the api may change significantly in the future.

## Sending requests

> Use the send method.

```javascript
http
    .send(
        'https://cloud.enklu.com:10001/v1/versions/web',
        { method: 'GET' })
    .then(function(res) { log.info(res.ok); })
    .fail(function(res) { log.info('Request failed: ' + res.text()); });
```

> Pull off the body of the response.

```javascript
http
    .send(
        'https://cloud.enklu.com:10001/v1/versions/web',
        { method: 'GET' })
    .then(res => {
        var json = JSON.parse(res.text());

        log.info('Current web version is ' + json.body.version);
    });
```

Send requests using the `send` function. The HTTP method must be provided, but a body may be included.

Enklu currently only supports ES5 so the [async and await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) keywords are not available. However, the interface closely resembles the web technology specification by using the `then` and `fail` functions to chain handlers onto the request. These methods should be used to handle the response.

## Shortcut Methods

> Send a get request. Currently, an object must be passed along for parameters.

```javascript
http.get('https://cloud.enklu.com:10001/v1/versions/web', {})
```

> Use post-- and don't forget the body.

```javascript
http.post('https://cloud.enklu.com:10001/v1/email/signin', { body: { email: 'foo@bar.com', password: 'fizzbuzz' } })
```

```javascript
http.put(url, { body: { name: 'testing' } })
```

> Delete also requires an object for parameters, but may be left empty.

```javascript
http.delete(url, {});
```

The HTTP API also includes shortcut methods for the common HTTP verbs: `get`, `post`, `put`, and `delete`. Other verbs may be available in the future.

## Request Parameters

> The method specifies which HTTP method to use. This is overwritten when using a named method, like `get` or `post`.

```javascript
http.send(url, { method: 'POST' })
```

> The body is an optional parameter that accepts a payload. This will be JSON encoded.

```javascript
http.send(url, { method: 'POST', body: { foo: 'bar' }});
```

Requests currently support two properties: `method` and `body`.

## Response Parameters

> Use the `ok` or `status` properties to check for success.

```javascript
.then(function(res) {
    log.info(res.ok);       // true
    log.info(res.status);   // 200
})
```

> Pull out the response text with `text()` and parse to consume.

```javascript
.then(function(res) {
    var json = JSON.parse(res.text());

    // do stuff
})
```

The HTTP response object contains information necessary for processing the HTTP response.