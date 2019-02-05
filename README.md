# Introduction

## Basic Concepts 

WebSockets The Bluzelle database communicates over WebSockets. Given an entry point \(such as `ws://bernoulli.bluzelle.com:51010`\), all communication happens over messages sent back and forth over the WebSocket.

## Consensus 

Being a decentralized database necessitates a system capable of synchronizing the states across the many nodes of the swarm. This is the responsibility of the consensus algorithm. Bluzelle currently implements the PBFT algorithm. 

