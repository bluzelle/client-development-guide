# Layer 6: Nonce, Timestamp, UUID

### Background

All database requests contain a header with a timestamp, nonce, and database uuid. The header is mirrored in any replies from the daemon. 

### Specifications

* The client must guarantee that no two requests have identical headers. This is the purpose of the nonce.
* The timestamp is the current time \(UTC, milliseconds to unix epoch\).
* The UUID should be a user-supplied v4 UUID, or else give an error.
* All subsequent layers are interpreted as request-response and should filter responses to match the nonce of the relevant request.



