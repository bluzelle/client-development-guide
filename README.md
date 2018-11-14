# Introduction

Basic Concepts WebSockets The Bluzelle database communicates over WebSockets. Given an entry point \(such as ws://testnet.bluzelle.com:51010\), all communication happens over messages sent back and forth over the WebSocket.

Consensus Being a decentralized database necessitates a system capable of synchronizing the states across the many nodes of the swarm. This is the responsibility of the consensus algorithm. Bluzelle currently implements the RAFT algorithm with plans to upgrade to PBFT. Numerous internet resources are available for learning about RAFT and other consensus algorithms.

Redirection Consensus algorithms hinge on a single node being named the leader. The leader is responsible for publishing changes to the database and is elected in way so to not compromise the decentralized nature of the database. If a client wishes to publish a change to the database \(i.e. through the commands create, update, or delete\), it must be through a direct WebSocket to the leader. The leader can change with time, so the client should be capable of following redirect responses.

