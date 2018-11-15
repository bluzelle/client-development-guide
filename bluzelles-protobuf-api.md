# Bluzelle's Protobuf API

The Bluzelle architecture consists of a request-response system through WebSockets.

Database requests and responses are [protobuf](https://github.com/google/protobuf) serial output. The master database protofile is given [here](https://github.com/bluzelle/swarmDB/blob/devel/proto/database.proto). The message `bzn_envelope` is the master container for both requests and responses. 

{% hint style="info" %}
Note: While not restricted by protobuf, the maximum size for keys is 4kB and the maximum size for values is 256kB.
{% endhint %}

## Requests and Responses

1. Generate a `bzn_envelope` object as specified in [database.proto](https://github.com/bluzelle/swarmDB/blob/devel/proto/database.proto). \(Please see the [protobuf documentation](https://developers.google.com/protocol-buffers/) for details on using protobuf in your language\)
2. Serialize the message.
3. The response, coming as raw bytes, should be interpreted as a `database_response`.

## Message Envelopes

There are instances where messages must be signed in either direction. The Bluzelle protobuf specification uses in place of messages their serializations \(these fields are denoted "...payload" and have types `bytes`\). It is necessary to embed the serializations directly because protobuf serializations are non-deterministic; signed messages cannot be recreated for the verification on the other end. 

Alongside the payload, there is a signature and public key.

Bluzelle uses ECDSA keys along the curve `secp256k1`.

