# Layer 7: API Layer

{% hint style="info" %}
v0.4.x Bernoulli _might_ include subscription.
{% endhint %}

### Background

As the top level of the library, the API layer provides the final mapping from protobuf requests & responses to the end-user interface.

### Specification

* Outgoing messages may only be of the `database_request` type.
* Incoming messages may only be of the `database_response` type.
* Requests and responses should always be matched up \(ie. a `read_request` receives a `read_response`\).
  * Subscription requests are the exception. They receive many responses of the type `subscription_update` until terminated with a separate unsubscribe request.
* The API may be blocking or non-blocking while asynchronous calls are being made.
* A the outcome of a request that does not receive a response is undefined. 
  * If the request is mutating, the database state is undefined until it receives a response.

