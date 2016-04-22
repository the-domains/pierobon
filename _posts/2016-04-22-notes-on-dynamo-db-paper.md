---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
keywords: []
description: Dynamo
datePublished: '2016-04-22T19:30:48.357Z'
dateModified: '2016-04-22T19:27:43.957Z'
title: ''
author: []
sourcePath: _posts/2016-04-22-notes-on-dynamo-db-paper.md
published: true
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
starred: false
url: notes-on-dynamo-db-paper/index.html
_type: Article

---
**Dynamo**
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/4df22113-1897-4516-bc0a-eb1cd8b54c6a.jpg)

Traditionally production systems store their state in relational databases. For many of the more common usage patterns of state persistence, however, a relational database is a solution that is far from ideal. 

Dynamo has a simple key/value interface, is highly available with a clearly defined consistency window, is efficient in its resource usage, and has a simple scale out scheme to address growth in data set size or request rates. Each service that uses Dynamo runs its own Dynamo instances. 

Data replication algorithms used in commercial systems 

traditionally perform synchronous replica coordination in order to 

provide a strongly consistent data access interface. To achieve this 

level of consistency, these algorithms are forced to tradeoff the 

availability of the data under certain failure scenarios. 

For systems prone to server and network failures, availability can 

be increased by using optimistic replication techniques, where 

changes are allowed to propagate to replicas in the background, 

and concurrent, disconnected work is tolerated. Need to decide 

**when**

and 

**who**

to resolve those conflicts. 

Many traditional data stores execute conflict resolution during writes and keep the read 

complexity simple. In such systems, writes may be rejected if 

the data store cannot reach all (or a majority of) the replicas at a given time. On the other hand, Dynamo targets the design space 

of an "always writeable" data store. 

Who: This can be done by the data store or the application. If conflict resolution is done by the data store, its choices are rather 

limited. In such cases, the data store can only use simple policies, such as "last write wins". Since the application is aware of the data schema it can decide on the conflict resolution method that is best suited for its client's experience. 

Other key principle in Dynamo design: 

Incremental scalability (no negative effect when adding 1 node) 

Symmetry (every node has the same responsibility) 

Decentralisation (use P2P) 

Heterogeneity (assign load to nodes according to their capacity) 

Dynamo is targeted mainly at applications that need an "always writeable" data store where no updates are rejected due to failures or concurrent writes. Dynamo is built for an infrastructure within a single administrative domain where all nodes are assumed to be trusted. applications that use Dynamo do not require support for hierarchical namespaces (a norm in many file systems) or complex relational schema (supported by traditional databases). Dynamo is built for latency sensitive applications that require at least 99.9% of read and write operations to be performed within a few hundred milliseconds. Dynamo can be characterized as a zero-hop DHT, where each node maintains enough routing information locally to route a request to the appropriate node directly. 

Dynamo treats both the key and the object supplied by the caller as an opaque array of bytes. It applies a MD5 hash on the key to generate a 128-bit identifier, which is used to determine the storage nodes that are responsible for serving the key. 

One of the key design requirements for Dynamo is that it must scale incrementally. This requires a mechanism to dynamically partition the data over the set of nodes (i.e., storage hosts) in the system. Dynamo's partitioning scheme relies on consistent hashing to distribute the load across multiple storage hosts. 

instead of mapping a node to a single point in the circle, each node gets assigned to multiple points in the ring. To this end, Dynamo uses the concept of "virtual nodes". A virtual node looks like a single node in the system, but each node can be responsible for more than one virtual node. 

Each data item is replicated at N hosts, where N is a parameter configured "per-instance". Each key, k, is assigned to a coordinator node (described in the previous section). The coordinator is in charge of the replication of the data items that fall within its range. 

In addition to locally storing each key within its range, the coordinator replicates these keys at the N-1 clockwise successor nodes in the ring. 

Dynamo treats the result of each modification as a new and immutable version of the data. It allows for multiple versions of an object to be present in the system at the same time. Most of the time, new versions subsume the previous version(s), and the system itself can determine the authoritative version (syntactic reconciliation). However, version branching may happen, in the presence of failures combined with concurrent updates, resulting in conflicting versions of an object. In these cases, the system cannot reconcile the multiple versions of the same object and the client must perform the reconciliation in order to collapse multiple branches of data evolution back into one (semantic reconciliation). 

Dynamo uses vector clocks in order to capture causality between different versions of the same object. A vector clock is effectively a list of (node, counter) pairs. One vector clock is associated with every version of every object. One can determine whether two versions of an object are on parallel branches or have a causal ordering, by examine their vector clocks. If the counters on the first object's clock are less-than-or-equal to all of the nodes in the second clock, then the first is an ancestor of the second and can be forgotten. Otherwise, the two changes are considered to be in conflict and require reconciliation. 

In Dynamo, when a client wishes to update an object, it must specify which version it is updating. This is done by passing the context it obtained from an earlier read operation, which contains the vector clock information. ![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/0b67021a-1752-415d-90df-fda677f86a25.jpg)
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/91928ad1-f224-4c7c-8819-5d18256351b9.jpg)