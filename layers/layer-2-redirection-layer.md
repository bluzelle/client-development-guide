# Layer 2: Redirection Layer

### Background 

All write-style requests should be directed to the swarm leader. This layer no longer resides in the client.

### Specifications

* The redirect field from an incoming database response should never be set.
* All subsequent layers should assume that a message response is never a redirection.

