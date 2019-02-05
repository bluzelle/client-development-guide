# Layer 4: API Layer

### Background

As the top level of the library, the API layer provides the final mapping from protobuf requests & responses to the API functions that the user calls.

### Specification

* Outgoing messages are of the`database_msg` or `status_request` types.
* Incoming messages are of the`database_response` or `status_response`types.
* Requests and responses are always matched up \(ie. a `read_request` receives a `read_response`\).
  * Any message can receive a `database_error`, which has an embedded error message.
* As shown in [database.proto](https://github.com/bluzelle/swarmDB/blob/devel/proto/database.proto), keys are strings and values are binary.
  * Keys should be enforced to 4kB and values to 256kB.
* The API should be non-blocking while asynchronous calls are being made \(there are usually language-dependent constructions for handling asynchronous behavior\).

