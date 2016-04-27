# Notes on efficient distributed Transactions.

By a distributed transaction, I mean the change of state of multiple distributed actors.
In case of failure of processing of that transaction by an actor, another set of actors may be chosen to perform that transaction. That introduces the problem of guaranteeing that only one transaction is perfomed. To do that, all actors need to be aware when a transaction is not valid. When the actor that originated the transaction guarantees that the previous transaction is invalid, he is able to retry.

One way to do that is to have timeouts in the transaction itself which when they are passed, that would make the transaction invalid. Two timeouts are possible. 

1. Local timeouts could allow the possible removal of an actor that misbehaves with another one without invalidating the transaction.
2. Global timeout is the timeout that makes the transaction invalid.

Timeouts require a common secure way to determine time. This can be done by creating such a secure service that distributed actors can interact with.

There can be many such secure services since we do not want to guarantee the order of the transactions themselves.


# Locally Ordered Distributed Transactions.

Specific actors might want to order their own part of their transaction. Having a specific time that is ordered across all their transactions is necessary for that. Keep in mind that each actor may have a different service with which it orders its transactions. All it needs to do is to put the signed time of that service on the transaction itself and pledge that it will not sign more transactions untill the current transaction is finished. If the transaction is performed, the singed time represents proof of the local order of the transaction.



Since both these types of time providers can replicated without loss of its properties, we are able to scale indefinitely as more transactions come into our system.
