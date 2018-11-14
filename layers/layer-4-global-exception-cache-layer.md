# Layer 4: Global Exception Layer

### Background

Incoming exceptions are communicated through the `error` field of the database response message.

Exceptions may be:

1. Specific to the request. For example, if you try to create a key that already exists, the error field will contain `"KEY ALREADY IN DATABASE"`.
2. Generic. If you try to send a message to the swarm while a consensus election is in progress, you will receive an error with `"ELECTION IN PROGRESS"`

Exceptions of the latter type are handled by sending the original message again without user input. Sending the message again necessitates the use of a cache.

### Specifications

* Possible generic messages are:
  * "ELECTION IN PROGRESS"
* Upon receipt of a generic error message, the client should send a duplicate message immediately or 100ms after the last send, whichever comes later.
* After three failed retries, the generic error passes through the layer with an indication of the retry failure.

