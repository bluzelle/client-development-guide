# Layer 3: Metadata Layer

### Background

All database requests contain a header with a nonce and database uuid. The header is mirrored in the daemon replies to this request.

* The client must guarantee that no two requests have identical headers. This is the purpose of the nonce.
* The UUID should be a user-supplied string. \(technically a v4 UUID, but this is not enforced\)
* This layer should filter responses to match the nonce of the relevant request.

### Specifications

* On outgoing messages:
  * Create a `database_header` message.
  * Fill in the uuid.
  * Generate a random 64-bit nonce.
    * Note that you may have to use a large-integer library or use bitwise arithmetic to fulfill all 64 bits.
  * Embed the header in the `database_msg`.
* On incoming messages:
  * Consider the nonce embedded within header.
  * Send this message to a handler specific to that nonce.
* All subsequent layers are interpreted as request-response. They assume that incoming messages are always replies to this particular request.
* Note that `status_request`'s and `status_responses` do not implement headers and skip this layer.



