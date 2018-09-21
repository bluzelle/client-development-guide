# PBFT Message Sequencing

With the introduction of PBFT, the request-response system is augmented to include two responses for write-style commands \(`create`, `update`, and `delete`\). The first response is a simple acknowledgement-style response to say that the message has been received. The second response is a commit response to say that the underlying database now reflects the change.

Refer to the following sequence diagram.

![](.gitbook/assets/screen-shot-2018-09-21-at-3.23.14-pm.png)

