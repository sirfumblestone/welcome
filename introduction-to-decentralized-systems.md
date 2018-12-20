# Introduction to the OrbitDB's underlying concepts 

OrbitDB is a decentralized database which uses [CRDTs](https://en.wikipedia.org/wiki/Conflict-free_replicated_data_type). It guarantees a (strong eventual consistency](https://en.wikipedia.org/wiki/Eventual_consistency) which is a [consistency model](https://en.wikipedia.org/wiki/Consistency_model) in computer science. 

Building systems in a decentralized manner introduces a lot of new concepts and trade-offs for a developer who has previously developed only centralized (but possibly distributed) systems. Before building anything it might be useful to first familiarize yourself with the essentials about distributed systems. This document aims to gather some good starting points for learning both distributed and especially, decentralized systems. It is also an attempt to explain some core mechanics of OrbitDB.

There is a difference between distributed and decentralized systems: decentralized system is a distributed system but it is not owned by one actor. On the other hand a distributed system can also be a centralized system and this is how most of the big systems today are.

## Where should I start learning?

Below are some good introductions to begin with:

- [A Thorough Introduction to Distributed Systems](https://medium.com/@stanislavkozlovski/a-thorough-introduction-to-distributed-systems-3b91562c9b3c) article by Stanislav Kozlovski is a short introduction to the distributed and decentralized space in general
- [Distributed systems for fun and profit](http://book.mixu.net/distsys/) is an online book by Mikito Takada that is easy to read and helps you to understand concepts like consistency, time and order, CRDTs and much more
- [Distributed Systems Theory for the Distributed Systems Engineer](https://www.the-paper-trail.org/post/2014-08-09-distributed-systems-theory-for-the-distributed-systems-engineer/) is a deeper dive into the subject, almost a curriculum by Henry Robinson

## OrbitDB Core Mechanics

### IPFS (Inter-planetary File System)

### Consensus

OrbitDB does not have consensus algorithm but it can be developed on top of it.

### Database synchronization 

### Replication vs duplication

**Replication**, only sends changes from a specified db to the other nodes.
**Duplication** is where each node has the full db with it, the same model exists e.g. in git or in a distributed ledger. This is accomplished in orbit by calling `db.load()` periodically in the app to make sure it has the latest and greatest.


Some notes from issue [#498](https://github.com/orbitdb/orbit-db/issues/498):

Somebody, somewhere, has to write the first entry into the CRDT and "start" the database.

pubsub handles the data transfer on the IPFS later and `load()` brings those contents into memory to be queried.

If I start a git repository and you eventually clone it, we both have a copy of the database locally, can perform operations on it, and have certain assurances that when we decide to sync up (which can be done at any time, at any interval), functions run on our data will yield the same outputs. The fact that I started it is just a consequence of the fact that somebody had to make the first commit.

In the case of OrbitDB, this DOES happen automatically via pubsub as you suggest, on the IPFS layer. It seems like you mostly don't like the fact you have to periodically call db.load to get the database into memory to work with, which is a valid critique and we can perhaps look at keeping a certain amount of data in memory at all times.
