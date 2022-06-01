* Introduction and preface
    * Develop a good intuition for what your systems are doing under the hood
    * Learn how to make data systems scalable (millions of users)
    * Highly available (minimal downtime) and operationally robust
    * Easier to maintain systems
    * Natural curiosity for how things work
    * Book is primarily about architecture, does not go into deployment, operations, security, management, etc.
* Chapter 1: reliability, scalability, maintainability
    * Reliability: tolerating hardware and software faults; human error
    * Scalability: measuring load and performance; latency percentiles and throughput
    * Maintainability: operability, simplicity, and evolvability
* Many applications need:
    * Store data (databases)
    * Remember result of expensive operation (caches)
    * Allow users to search data (search indexes)
    * Send message to another process for async handling (stream processing)
    * Periodically crunch a lot of data (batch processing)
* Data systems don’t neatly fall into buckets
    * Redis can be used as a message queue
    * Message queues like Kafka have database-like durability guarantees
* Keeping caches and search indexes in sync with the main database is the responsibility of application code
    * Create a new, special-purpose data system from smaller, general-purpose components
    * Guarantees: cache will be invalidated on write so that outside clients see consistent results
* Questions that arise:
    * Data remains correct when things go wrong
    * Consistent good performance when system parts degrade
    * How to scale
    * What is a good API
* Reliability
    * System should work correctly (with desired perf) even in face of adversity (hardware or software faults)
* Scalability
    * There should be ways of dealing with system growth
* Maintainability
    * Over time, engineers and operators should be able to work on the system productively
* Reliability
    * Continues to work correctly even when things go wrong
    * Faults: things that can go wrong
    * Systems that anticipate faults: fault-tolerant or resilient
    * Building reliable systems from unreliable parts
    * Fault: defined as one part of the system deviating from the spec
    * Not the same as failure: when the system stops providing the required service to the user
    * Netflix Chaos Monkey: example of deliberately causing faults to test a system’s fault-tolerance
    * Sometimes prevention is better than fault-tolerance: in the case of security breaches, which can’t be undone
* Hardware faults
    * Hard disks have a mean time to failure of about 10 to 50 years
    * Storage cluster with 10k disks has one disk die per day
    * First response: add redundancy
        * RAID disks
        * Dual power supplies for servers
        * Hot swappable CPUs
        * Backup power generators
    * More applications have begun using more machines, proportionally increasing rate of hardware faults
    * In AWS, virtual machines become unavailable without warning, since AWS prioritize elasticity over single-machine reliability
    * Single-server system requires downtime if you need to reboot the machine
    * System that tolerates machine failure can be patched one node at a time (called a rolling upgrade)
* Software errors
    * Software bug that causes every instance of an app server to crash given bad input
        * Example: leap second bug in Linux Kernel causing applications ot hang
    * Runaway process that eats up shared resource (CPU, memory, network bandwidth, disk space)
    * A service that the system depends on is slow, returns wrong responses, or becomes unresponsive
    * Cascading failures
    * These bugs are often caused by assumptions that are suddenly invalid
    * Possible solutions:
        * Thinking through assumptions
        * Testing
        * Process isolation
        * Restartable processes
        * Measuring and analyzing in production
* Human errors:
    * Configuration errors by operators were the leading cause of outages
    * Make it easy to do the right thing with well-designed abstractions, APIs, and admin interfaces
    * Provide sandbox environments so that people can play around without affecting real users
    * Test thoroughly
    * Allow quick and easy recovery (roll back configs, roll out new code gradually to users, provide tools to recompute data)
    * Set up monitoring: performance metrics, error rates (called telemetry in other disciplines)
    * Implement good management practices
* How important is reliability
    * Photo application containing valuable family photos is suddenly corrupt
    * Situation where we sacrifice reliability to reduce development cost (such as in a prototype), or operational cost (service has narrow profit margin)
    * But we should make these choices very carefully
* Scalability
    * …
