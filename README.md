# JSend
A simple PHP implementation of the [JSend specification](http://labs.omniti.com/labs/jsend).

## Usage
```php
use JSend\JSendResponse;
```

### New response
```php
$success = new JSendResponse('success', $data);
$fail = new JSendResponse('fail', $data);
$error = new JSendResponse('error', $data, 'Not cool.', 9001);
```

```php
$success = JSendResponse::success($data);
$fail = JSendResponse::fail($data);
$error = JSendResponse::error('Not cool.', 9001, $data);
```

**Note**: an `InvalidJSendException` is thrown if the status is invalid or if you're creating an `error` without a `message`.

### Convert JSendResponse to JSON
`__toString()` is overridden to encode JSON automatically.

```php
$json = $success->encode();
$json = (string) $success;
```

### Convert JSON to JSendResponse
```php
try {
    $response = JSendResponse::decode($json);
} catch (InvalidJSendException $e) {
    echo "You done gone passed me invalid JSend.";
}
```

### Doing something useful
```php
$isSuccess = $response->isSuccess();
$isError = $response->isError();
$isFail = $response->isFail();
$status = $response->getStatus();
$data = $response->getData();
```

Additionally, you can call the following methods on an error. A `BadMethodCallException` is thrown if the status is not `error`, so check first.

```php
if ($response->isError()) {
    $code = $response->getErrorCode;
    $message = $response->getErrorMessage;
}
```
