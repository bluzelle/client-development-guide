# Layer 3: Cryptographic Layer

### Background

Cryptography is employed for permissioning \(ex. who can modify databases\) and for database consensus.

{% hint style="info" %}
Only the first point below \(permissioning\) is to be implemented in Bernoulli, v0.4.x.
{% endhint %}

* Each client application has a public and private key-pair. Any message sent is signed and sent in an envelope. \(See [Bluzelle's Protobuf API](../bluzelles-protobuf-api.md#message-envelopes)\).
  * This process occurs for all outgoing messages.
* The PBFT consensus protocol requires that a proportion of participating nodes agree to an operation. Since a client cannot speak to every one of these nodes, the database response is signed multiple times, collated, and transmitted to the client application. 

  * The client must verify the single signature of the outer-level message envelope.
  * The client must verify the collated signatures of the `database_response` envelope.
  * The client must verify that the provided public keys are indeed members of the swarm via a third party such as Ethereum.
  * This process occurs for all incoming messages.

### Specifications

* All Bluzelle keys are ECDSA on the curve `secp256k1`.
* The client must accept a key-pair argument in a way that is reasonable to the application. For desktop applications, this will be through a `.pem` file on the filesystem. For web applications, it will be through an argument.
* All subsequent layers are assumed to be working with the interpreted payload.



