# WebSocket Architecture

In the Bluzelle database, there are two kinds of commands, a **write** command and a **read** command. Write commands are __**create**, **update**, and **delete**; read commands are all others.

When writing to the database, all write commands must be sent to the database _leader_. If a write command is not sent to the leader, you will receive a _redirection response_. \(See the [Protobuf API](bluzelles-protobuf-api.md)\)

It is not recommended to maintain a single persistent connection to the leader for performance reasons.  We recommend maintaining _two_ WebSocket connections, one for write-style commands \(primary\) and one for read-style commands \(secondary\). A flowchart explaining the connections is shown below. 

![](.gitbook/assets/screen-shot-2018-09-20-at-4.43.15-pm.png)

{% file src=".gitbook/assets/client-connections.pdf" %}



