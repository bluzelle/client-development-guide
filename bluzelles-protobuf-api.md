# Bluzelle's Protobuf API

The Bluzelle architecture consists of a request-response system through WebSockets.

Database requests and responses are [protobuf](https://github.com/google/protobuf) serial output. The master database protofile is given [here](https://github.com/bluzelle/swarmDB/blob/devel/proto/database.proto). The message `bzn_envelope` is the master container for both requests and responses. 

The protobuf specification files are found in the [proto directory](https://github.com/bluzelle/swarmDB/tree/devel/proto) of the Bluzelle daemon.

{% hint style="info" %}
Note: While not restricted by protobuf, the maximum size for keys is 4kB and the maximum size for values is 256kB.
{% endhint %}

{% hint style="info" %}
The WebSocket needs to be configured to send and receive binary data. This is a language-dependent.
{% endhint %}

## Requests and Responses

Please see the [protobuf documentation](https://developers.google.com/protocol-buffers/) for details on using protobuf in your language.

### Sending a database request

1. Generate a `database_msg` in [database.proto](https://github.com/bluzelle/swarmDB/blob/devel/proto/database.proto) with the appropriate fields.
2. Generate a `bzn_envelope` object as specified in [bluzelle.proto](https://github.com/bluzelle/swarmDB/blob/devel/proto/bluzelle.proto).
3. Serialize the `database_msg` and put that in the `database_msg` field in the `bzn_envelope`.
4. Fill in the cryptographic information in the `bzn_envelope` \(details in [Layer 2: Cryptographic Layer](layers/layer-2-cryptographic-layer.md)\).
5. Serialize the `bzn_envelope` and send it over the wire.

### Receiving a database response

1. The response should be interpreted as a `bzn_envelope`.
2. All cryptographic fields can be ignored. \(They will be implemented in v0.5.x via signature collation\)
3. A payload will be set to `database_response`. Interpret the binary as a `database_response` message in [database.proto](https://github.com/bluzelle/swarmDB/blob/devel/proto/database.proto).

### Sending a status request

1. Generate a `status_request` object in [status.proto](https://github.com/bluzelle/swarmDB/blob/devel/proto/status.proto).
2. Generate a `bzn_envelope` object as specified in [bluzelle.proto](https://github.com/bluzelle/swarmDB/blob/devel/proto/bluzelle.proto).
3. Serialize the `status_request` and put that in the `status_request` field in the `bzn_envelope`.
4. Ignore the cryptographic information. 
5. Serialize the `bzn_envelope` and send it over the wire.

### Receiving a status response

1. The response should be interpreted as a `bzn_envelope`.
2. All cryptographic fields can be ignored.
3. The payload will be set to `status_response`. Interpret the binary data as a `status_response` message in [status.proto](https://github.com/bluzelle/swarmDB/blob/devel/proto/status.proto).

## Message Envelopes

Clients need to sign messages for security reasons. For signed messages, instead of directly embedding them inside of parent messages, the Bluzelle protobuf specification embeds their serializations \(these fields are denoted "...payload" and have types `bytes`\). It is necessary to embed the serializations because protobuf serializations are non-deterministic; signed messages cannot be recreated for verification on the other end. 

Alongside the payload, there is a signature and public key.

Bluzelle uses ECDSA keys along the curve `secp256k1`.

See [Layer 2: Cryptographic Layer](layers/layer-2-cryptographic-layer.md) for more on signing.

