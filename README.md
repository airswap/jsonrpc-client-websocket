# JSON-RPC 2.0 Client over WebSockets

Forked from https://github.com/fabriciobastian/jsonrpc-client-websocket

## Open connection

```typescript
const websocketUrl = 'ws://mywebsocketurl:port';
const requestTimeoutMs = 2000;
const websocket = new JsonRpcWebsocket(websocketUrl, requestTimeoutMs, (error: JsonRpcError) => {
  /* handle error */
});
await websocket.open();
```

Requests that do not receive a response within the specified timeout will fail with a REQUEST_TIMEOUT code.
The callback (optional) is used for eventual errors, such as receiving a response that does not match any request id and
connection errors. Furthermore, all errors that are sent to an eventual caller are also reported on the callback, e.g.
if an rpc method is called with an invalid number of parameters, etc...

## Close connection

```typescript
await websocket.close();
```

## Call RPC method

Considering that the server has a method `sum(a: int, b: int)`

### with positional parameters

```typescript
websocket
  .call('sum', [1, 2])
  .then((response) => {
    // handle response
  })
  .catch((error) => {
    // handle error
  });
```

### with named parameters

```typescript
websocket
  .call('sum', { b: 1, a: 2 })
  .then((response) => {
    // handle response
  })
  .catch((error) => {
    // handle error
  });
```

## Send notification

Considering that the server has a method `log(message: string)`

```typescript
websocket.notify('log', ['a log message']);
```

## Define RPC method

```typescript
websocket.on('sum', (a: number, b: number) => {
  return a + b;
});
```

The defined RPC methods can also be called with both positional and named parameters.

## Remove RPC method

```typescript
websocket.off('sum');
```
