# Layer 5: Max Size Layer

### Background

Key and value fields in the bluzelle database are limited to 4kB and 256kB, respectively. Protobuf does not contain a mechanism to specify or enforce this constraint.

### Specification

* Limit the size of all keys to 4kB and all values to 256kB. Throw an error if one or both of these values exceeds the limit.
* The swarm will give an error response if either field is too large; however, this should never happen as the responsibility is on the client.

