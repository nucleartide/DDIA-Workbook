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
    * A system working reliably today may not work reliably in the future
    * One reason: increased load (concurrent users, volume of data)
    * Scalability is not 1-dimensional; if a system scales in a particular way, what are the options to handle
    * Defining load
        * Load parameters (requests per second to a web server, ratio of reads to writes in a database, number of simultaneous users in a chat room, hit rate on cache, etc.)
        * Is average case important? Or small number of extreme cases?
    * Consider Twitter
        * Operation: post tweet (4.6k requests per second on average, over 12k requests per second at peak)
        * Operation: view tweets posted by followed people (300k requests per second)
        * Fan out: term from electronic engineering, where output needs to drive all attached inputs
        * Ways of implementing (insert into global collection of tweets, maintain a cache for each user’s home timeline)
    * First version of Twitter used approach 1 (SQL query), but had to move to approach 2 (since writes are 2 orders of magnitude fewer than reads)
    * Twitter tries to deliver tweets to followers within 5 seconds
    * Load parameter for Twitter: distribution of followers per user (weighted by how often users tweet)
    * Exception for celebrities: tweets for celebrities are merged into a user’s home timeline
* Describing performance
    * When you increase a load param, and keep system resources unchanged, how is system performance affected?
    * When you increase a load param, how much do you need to increase system resources to keep performance unchanged?
    * Throughput: number of records processed per second, important in batch processing systems
    * Response time: more important in online systems
    * Skew: data not spread evenly across worker processes, needing to wait for the slowest task to complete
    * Latency vs response time
        * Response time is what the client sees
        * Latency is the duration that a request is waiting to be handled, during which it is latent
    * Think of response time as a distribution of values to measure
        * There are occasional outliers in response time
    * Average response time, typically the arithmetic mean, but not good metric if you want to know the “typical” response time
    * Better to use percentiles; median is a good metric for how long users typically wait (abbreviated as p50)
    * P95, p99, p999 to look at outliers – these are known as tail latencies
    * P999 is important for Amazon, since those outliers are typically the customers who buy the most stuff
    * On the other hand, P9999 wasn’t worth pursuing for Amazon
    * Service level objective (SLO) and service level agreement (SLA)
        * Often uses percentiles
        * Define the expected performance and availability of a service
        * Example: service is up if median response time (p50) is less than 200ms, and 99th percentile is under 1s, service must be up at least 99.9% of the time
            * Customers can demand a refund if SLA is not met
    * Head-of-line blocking: small number of slow requests will hold up processing of subsequent requests
        * Due to this effect, it’s important to measure response times on client side
    * When generating load artificially, load-generating client needs to send requests independently of response time
        * Don’t want to artificially keep the queues shorter than they would be in reality, which skews measurements
