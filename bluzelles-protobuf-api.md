# Bluzelle's Protobuf API

The Bluzelle architecture consists of a request-response system through WebSockets.

Database requests are [protobuf](https://github.com/google/protobuf) serial output embedded in JSON. The master database protofile is given [here](https://github.com/bluzelle/swarmDB/blob/devel/proto/database.proto). The protobuf message type for requests is `database_msg`.

Database responses are direct serial output through WebSocket. The protobuf message type for responses is `database_response`.

## The API lifecycle

1. Generate a `database_msg` object as specified in [database.proto](https://github.com/bluzelle/swarmDB/blob/devel/proto/database.proto).
2. Serialize the message.
3. Convert the serial data to a base64 string.
4. Embed the base64 string into `"{ "bzn-api": "database", "msg": "YOUR STRING HERE" }"`.
5. Establish a WebSocket connection to a Bluzelle node and send the message as a string.
6. The response, coming as raw bytes, should be interpreted as a `database_response`.

## Example messages with `bluzelle-js`

```text
// uuid: '8078e15c-ac47-4db9-83df-da6dba71231a'

bluzelle.create('hello', 'world');

{
    "bzn-api":"database",
    "msg":"Uj0SKAokODA3OGUxNWMtYWM0Ny00ZGI5LTgzZGYtZGE2ZGJhNzEyMzFhEARSERIFaGVsbG8aCAEid29ybGQi"
}



bluzelle.read('myKey');

{
    "bzn-api":"database",
    "msg":"UjMSKAokODA3OGUxNWMtYWM0Ny00ZGI5LTgzZGYtZGE2ZGJhNzEyMzFhEAZaBxIFbXlLZXk="
}



bluzelle.remove('hello');

{
    "bzn-api":"database",
    "msg":"UjMSKAokODA3OGUxNWMtYWM0Ny00ZGI5LTgzZGYtZGE2ZGJhNzEyMzFhEAdqBxIFaGVsbG8="
}



bluzelle.has('abcd');

{
    "bzn-api":"database",
    "msg":"UjISKAokODA3OGUxNWMtYWM0Ny00ZGI5LTgzZGYtZGE2ZGJhNzEyMzFhEAlyBhIEYWJjZA=="
}

```

