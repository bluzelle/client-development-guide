# Layer 1: Persistent Connection

### Background

This layer is responsible for establishing and maintaining a WebSocket connection.

### Specification

* The WebSocket should send and receive binary messages. 
* All subsequent layers should assume a persistent connection.

{% hint style="info" %}
Future versions of client libraries may utilize a connection pool to distribute reliance on a single connections.
{% endhint %}

