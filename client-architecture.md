# Client Architecture

This guide is designed as a layered model, with each layer providing an invariant or service to the layer above. While it is not necessary for a client to use this model, it is the recommended architecture for client applications, and will be the means of presentation for this guide.

### Handling Errors

Errors in a client library may come from two sources:

1. Internal library logic.
2. Daemon behavior inconsistent from its specification.

The first case should be communicated as a regular error in the context of the programming environment.

The second case should be communicated within the proper context of the API. A failed assertion should be returned from the API call with the reason for failure, such as an improper response type or an unset field. The client application itself should not fail due to daemon malbehavior.

### Handling Errors in bluzelle-js

bluzelle-js incorporates error handling for daemon invariants the following way: at each layer, if an assertion for an incoming message fails, the payload of the `response` oneof inside of `database_response` in `database.proto` is replaced with a `database_error` that includes a message of the problem.

At the API layer, all error messages are then communicated from the API call with a promise rejection. Error handling is therefore generic over daemon-sourced errors, like creating a duplicate key; and daemon malbehavior caught in the client, like an incorrect response type.

