# Client Architecture

This guide is designed as a layered model, with each layer providing an invariant or service to the layer above. While it is not necessary for a client to use this model, it is the recommended architecture for client applications, and will be the means of presentation for this guide.

### Handling Errors

Errors in a client library may come from two sources:

1. Internal library logic.
2. An error communicated from the daemon. \(ie. creating a duplicate key\)
3. Daemon behavior inconsistent from its specification. \(ie. a missing protobuf field\)

The first case should be communicated as a regular error in the context of the programming environment.

The second and third cases should be communicated within the proper context of the API. Errors should be returned from the API call with the reason for failure. The client application itself should not fail due to daemon errors or malbehavior.

### Handling Errors in bluzelle-js

Messages in the second case are valid protobuf messages and propagate through the layers as any other. At the API layer, all error messages are then communicated from the API call with a promise rejection.

Errors in case three are handled the following way: at each layer, if an assertion for an incoming message fails, the payload is replaced with an error that includes a message of the problem. Error handling is therefore generic over daemon-sourced errors, like creating a duplicate key; and daemon malbehavior caught in the client, like an incorrect response type.

