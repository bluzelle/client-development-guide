# Layer 3: Redirection Layer

### Background 

All mutating requests should be directed to the swarm leader. This layer no longer resides in the client.

### Specifications

* The redirect field from an incoming database response should never be set.
* All subsequent layers should assume that the redirect field is not set.

