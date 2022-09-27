* Building fault-tolerant distributed systems
* Seek abstractions that allow an application to ignore some of the problems with distributed systems
* Consensus: getting all of the nodes to agree on something
* Examples:
    * Database with single leader replication
* Split-brain: if two nodes believe that they are the leader, which leads to data loss
* Understanding of the scope and limits of guarantees and abstractions that can be provided in a distributed system
* Eventual consistency: if you stop writing to the database and wait for an unspecified length of time, then all reads will eventually return the same value
    * Another name is convergence: all replicas eventually converge to the same value
    * Does not specify when replicas will converge
    * Hard for application developers, because EC (abbreviated) is different from the behavior of variables in a single-threaded program
* When working with a database with weak guarantees, you mustn’t assume too much
    * Edge cases only appear when there are network faults, or at high concurrency
* Systems with stronger guarantees may have worse performance, or be less fault tolerant than those with weaker guarantees
* There are similarities with transaction isolation levels, but distributed consistency is about coordinating replica state in face of delays and faults
* 3 topics:
    * Linearizability, and its pros and cons
    * Ordering events in a distributed system, causality, total ordering
    * How to atomically commit a distributed transaction, which leads to solutions for the consensus problem
* Linearizability
    * Make a system appear as if there were only one copy of the data
    * All operations are atomic, even though there may be multiple replicas in reality
    * Linearizability is a recency guarantee: the most recently written value is read
    * (Non-linearizable example of writing high scores into replica databases, with Alice and Bob. Bob reads a stale high score
* What makes a system linearizable?
    * Writing the same key x in a linearizable database; x is called a register
    * Register can be a key in a key-value store, a row in a relational database, a document in a document database
    * Client does not know when database processed the request; it only knows when the request was sent, and when the response was received
* Stopped on page 326, beginning. Start over at end of page 325.
