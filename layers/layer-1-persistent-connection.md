# Layer 1: Persistent Connection

### Background

In the event that the WebSocket connection is disrupted, a new connection should be re-established to the same node without user intervention.

### Specification

* If an existing message fails to connect, attempt to re-establish a connection and treat all subsequent layers as being persistent.
* If a connection cannot be re-established, relay an error message to the user, such as "node died".
* All subsequent layers should assume a persistent connection.

{% hint style="info" %}
Future versions of client libraries may utilize a connection pool so to not rely on a single connection nor a single node. 
{% endhint %}

