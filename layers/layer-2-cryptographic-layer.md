# Layer 2: Cryptographic Layer

### Background

Cryptography is employed for permissioning \(ex. who can modify databases\) and for database consensus.

* Each client application has a public and private key-pair. Any message sent is signed and sent in an envelope. \(See [Bluzelle's Protobuf API](../bluzelles-protobuf-api.md#message-envelopes)\).
  * The private key is an argument to the Bluzelle client. 
  * The public key can be generated from the private key.

### Specifications

* All Bluzelle keys are ECDSA on the curve `secp256k1`.
* The client must accept a key argument in a way that is reasonable to the application. For desktop applications, this may be through a `.pem` file on the filesystem. For web applications, it may be through an argument.

### Outgoing Messages

1. Given a completed message that is a `database_msg` or `status_request`, serialize it and embed it as the appropriate payload type in the `bzn_envelope`. 
2. If the message was a `status_request`, we are done. Otherwise, continue to the signing process.
3. Fill in the `timestamp` field with current time as milliseconds since UNIX epoch \(seen [here](https://currentmillis.com/)\).
4. Fill in the `sender` field with the public key in PEM format. It should look like this: "MFYwEAYHKoZIzj0CAQYFK4EEAAoDQgAEY6L6fb2Xd9KZi05LQlZ83+0pIrjOIFvy0azEA+cDf7L7hMgRXrXj5+u6ys3ZSp2Wj58hTXsiiEPrRMMO1pwjRg=="
5. The last step is to fill in the signature. Follow these steps closely:
   1. Take the sender, payloadCase \(you should be able to get it from protobuf\), payload, and timestamp. 
   2. Encode them as ascii.
   3. For each ascii string, create a new string that contains its length, a pipe character, then the string itself. For example, the timestamp would be `13|1549394321578`.
   4. Concatenate all of the strings in the same order that they were given. Empty message example: "120\|MFYwEAYHKoZIzj0CAQYFK4EEAAoDQgAEHLmWG+xY1lZak68iQMFCh2Vm1EfkEOkbclWWbEO1s+qpf6D6D/Yjo9CR2/zLHWkgTqo71nnjWWEU5FekTHjVmQ==2\|100\|1\|0"
   5. Convert this string to binary with ascii as the encoding.
   6. Sign the binary with the ECDSA private key.
   7. Embed the signature in the protobuf file.

### Incoming Messages

* Cryptography is not verified for incoming messages. Interpret the payload as a `database_response` or `status_response` and continue to higher layers.





