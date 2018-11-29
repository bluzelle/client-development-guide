# Client Architecture

This guide is designed as a layered model, with each layer providing an invariant or service to the layer above. While it is not necessary for a client to use this model, it is the recommended architecture for client applications, and will be the means of presentation for this guide.

### Handling Errors

Errors in a client library may come from two sources:

1. Internal library logic.
2. Daemon behavior inconsistent from its specification.

The first case should be communicated as a regular error in the context of the programming environment.

The second case should be communicated within the proper context of the API. A failed assertion should be returned from the API call with the reason for failure, such as an improper response type. The client application should not fail due to inconsistent daemon behavior.

