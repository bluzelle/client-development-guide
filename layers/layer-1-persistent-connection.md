# Layer 1: Persistent Connection

### Background

In the event that the WebSocket connection is disrupted, a new connection should be re-established to the same node without user intervention.

### Specification

* If a connection closes, attempt to re-establish a connection.
* If a connection cannot be re-established after retrying, relay an error message to the user, such as "node died".
* If a request is sent after connection failure and prior to re-establishment, it should be cached and sent after re-establishment. 
* If a request was sent prior to connection failure, no reply was received, and it was within 200ms of connection failure, the message response is undefined.
  * For non-mutating requests, the message should be resent after re-establishment.
  * For mutating requests, the database state is undefined. The behavior in this circumstance is currently undefined.
* All subsequent layers should assume a persistent connection.

{% hint style="info" %}
Future versions of client libraries may utilize a connection pool so to not rely on a single connection nor a single node. 
{% endhint %}

{% hint style="info" %}
Post-Bernoulli, connections must also re-establish subscriptions.
{% endhint %}



