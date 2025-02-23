---
layout: post
title:  "Tackling CRDTs and Brain Splits in Distributed Systems. How does Elixir handle it?"
date:   2023-06-20 00:18:23 +0700
categories: [architecture, CRDT, brain split]
---


In distributed systems and multi-user applications, handling concurrent updates on replicated data can be a major headache. Network partitions, intermittent connectivity, and simultaneous edits often force engineers to write complicated conflict resolution logic or rely on heavy coordination protocols.

Conflict-free Replicated Data Types (CRDTs) offer a simpler, more robust approach. As per the CRDT.tech resources, a CRDT is a data structure designed for optimistic replication: multiple replicas can independently make updates—even offline—and still automatically merge to a correct, consistent state once reconnected. This eliminates the need for special-case conflict resolution code or user intervention.

## Typical Scenarios That Need CRDTs

Mobile apps with local data (calendars, notes, contacts) that later sync to the cloud.

Distributed databases that keep multiple replicas in different regions for fault tolerance and high availability.


Collaborative software (e.g., Google Docs, Figma), where multiple users simultaneously modify shared files.

Large-scale data storage setups that replicate data globally for performance and reliability.

## The Split-Brain (Brain-Split) Challenge

A notorious problem in distributed systems is the split-brain scenario. This happens when a network partition (or severe communication failure) divides a cluster into two or more subsets—each “subset” believes it is fully in charge of the data, continuing to accept writes. When the network heals, these now-divergent replicas must merge.

**Risk of Conflicts** In a traditional system, you’d face data overwrites or partial merges where one “brain” discards changes made by the other.

**Data Loss** If a system automatically favors one side (“the winner”), the losing side may lose user updates entirely.

**Inconsistent State** Even after rejoining, data might remain out of sync, causing bugs and confusion for end-users.

CRDTs address these challenges gracefully. They allow concurrent updates on all partitions (even in a split-brain scenario) and automatically merge them back into a single coherent state—no manual conflict resolution needed.
CRDT Implementations

If you’re ready to try out CRDTs, check out CRDT Implementations. You’ll find libraries and frameworks in various languages (e.g., JavaScript, Go, Erlang, Rust) that implement counters, sets, maps, and sequence data structures with built-in conflict-free merges.
Use Cases

## Handling of brain splits in Elixir

### Mnesia Database Netsplits

Mnesia, a distributed database bundled with Erlang/OTP, can replicate tables across multiple nodes. 
In an Elixir application using Mnesia in a cluster of nodes, a network partition can cause:

- Each partition of the cluster continues to accept writes independently.
- When the network heals, Mnesia must somehow reconcile conflicting changes.

**Example**

- Cluster Setup: An Elixir app uses Mnesia for session storage across three nodes: node_a, node_b, node_c.
- Partition Occurs: node_a and node_b remain connected to each other, but node_c is isolated.
- Writes on Both Sides: A user updates a session on node_a (replicated to node_b), while another update arrives on node_c.
- Rejoining: Once node_c reconnects, Mnesia sees conflicting session changes.
- Conflict Resolution: Depending on the table type (e.g., disc_copies) and conflict-resolution strategy, Mnesia could choose last write wins or force you to run a custom merge function.

**Resource**

- [Official Erlang Mnesia Docs: Handling network partitions](https://www.erlang.org/doc/apps/mnesia/mnesia_chap7.html#recovery-from-communication-failure)
   
   
### Phoenix Presence and Split-Brain

Phoenix Presence tracks user presence (e.g., who’s online, which channel a user belongs to) across a distributed Phoenix cluster. By default, Presence uses the Phoenix.PubSub system running on Erlang’s distribution. If there’s a net-split:

- Each isolated subset of nodes thinks it still has the entire cluster.
- Users connected to each subset might show up as “online” in two different presence trackers.
- After a partition heals, the presence metadata from both subsets must reconcile.

**Example**

- Two-Node Phoenix: node_a and node_b run the same Phoenix application with shared Presence.
- Network Split: A glitch isolates the nodes for several minutes. Each node sees half the user population.
- Stale Online Lists: If users moved or disconnected on the other node, the local node wouldn’t know.
- Merge: Once reconnected, Presence merges track states. If the system is well-configured, it’ll end up with a consistent global presence list. However, if not, it might see phantom online statuses or double entries.

**Resource**

- [Phoenix Presence Docs](https://hexdocs.pm/phoenix/Phoenix.Presence.html#content) 

### Horde / Swarm for Distributed Process Registry

Horde and Swarm are Elixir libraries that manage distributed process registries and dynamic supervision trees. They handle automatic failover of processes when nodes go down or come up. However, in a net-split:

- Each subset of the cluster might spawn its own “singleton” process (believing it’s the only one).
- Both sides can keep track of different cluster topologies.
- When the network is restored, the library must detect the duplicate processes and terminate all but one.

**Example**

- Horde Registry: Horde is used to ensure only one instance of MyWorker runs cluster-wide.
- Partition: Node A and Node B get separated. Both think they’ve lost the other.
- Duplicate Processes: Each side starts (or continues) running MyWorker because each believes it’s responsible.
- Rejoin: Horde uses vector clocks or versioning to see which MyWorker is newest or how to unify state. One worker is gracefully shut down, and the other remains.

**Resources**

- [Horde Docs: Handling netsplits in dynamic clusters](https://hexdocs.pm/horde/readme.html).
- [Swarm Docs: Partition healing and conflict resolution](https://hexdocs.pm/swarm/readme.html).

### Riak Core-like Architectures (with Elixir Ports)

Though originally Erlang-based, Riak Core provides a ring-based architecture for building AP systems that can tolerate partitions. Elixir developers have experimented with ports or forks of Riak Core. If your Elixir application is built on a ring-based model with eventual consistency:

- Split-brain means multiple vnodes (virtual nodes) can end up storing partial data replicas.
- Each side can keep accepting writes for the same key range.
- Once rejoined, the system merges via sibling resolution or CRDT-based merges (Riak Core famously used CRDTs for some data types).

**Example**

- Elixir Service: An Elixir-based system uses a Riak Core-inspired ring to distribute data shards.
- Partition: The ring is split in half. Each half handles writes for overlapping key ranges.
- Merger: After reconnection, sibling objects (e.g., two versions of the same data) must be reconciled, often with a last-write-wins or CRDT approach.

**Resource**

- Legacy Riak Core (Erlang) docs – concept of “vnodes” and ring membership. 


### Custom Net-Split Handling in Erlang/Elixir Clusters

Some teams build custom logic to detect node connectivity changes. The Erlang runtime provides the :net_kernel and :erlang.monitor_node/2 functions to track when a node goes up or down. If a flurry of node-down messages can be seen, there might be a net-split. The application can:

- Enter a “degraded” mode.
- Stop certain global services or write operations.
- Retry or buffer writes until the cluster is healthy again.

If there’s an actual partition, each half will do something different. On reconnection, states must be unified. This is effectively a manual approach to dealing with a potential brain-split.

**Example**

- Monitoring: You :erlang.monitor_node/2 each node in the cluster.
- Detection: When Node B sees Node A go down, it sets a global variable :split_brain_detected = true.
- Local Writes Only: Node B queues external interactions or logs them. Node A does the same.
- Healing: When Node A is detected “up” again, a custom “merge protocol” merges logs or runs a conflict resolver.

**Resources**

- [Erlang :net_kernel module](https
- [Erlang :erlang.monitor_node/2 function](https://erlang.org
- [Erlang Distribution Docs](https://erlang.org/doc/reference_manual/distributed.html)
- [How to deal with partial netsplits?](https://elixirforum.com/t/how-to-deal-with-partial-netsplits/69060)


## Testing with Chaos

Tools like [PropEr](http://proper.softlab.ntua.gr/) (property-based testing in Elixir/Erlang) or Chaos Engineering approaches can inject network failures. This ensures an app can handle partial disconnections gracefully.


## Use Cases for CRDTs

### Collaborative Editing

Example: Google Docs, Microsoft Office 365
Description: CRDTs power real-time collaboration by letting multiple users edit documents simultaneously—even offline. Each user’s local copy can diverge, then seamlessly converge when connected again.
Benefit: Provides a fluid, real-time experience where conflicts are transparently resolved. No central server arbitration is necessary to prevent inconsistent states.

### Messaging and Chat Applications

Example: Slack, WhatsApp, Microsoft Teams
Description: CRDTs ensure consistent replication of messages, edits, or deletions across clients. If a user goes offline, local message changes are integrated correctly upon reconnection.
Benefit: Ensures everyone sees a coherent conversation, regardless of network reliability or client offline periods.

### Distributed File Systems

Example: Dropbox, Google Drive
Description: CRDTs can track file versions across distributed nodes. Users can work offline; when they reconnect, file modifications merge automatically.
Benefit: Minimizes data conflicts and version chaos, ensuring users have the most up-to-date and consistent file copies.

### E-Commerce Platforms

Example: Amazon, eBay
Description: Distributed inventory systems, shopping carts, and order processing benefit from CRDT-based merges when updates happen in multiple regions.
Benefit: Prevents double-selling, stale cart states, or lost orders by automatically reconciling concurrent modifications to inventory or user sessions.

### Social Media Applications

 Example: Facebook, Twitter
Description: User posts, comments, and interactions (likes, shares) replicate across multiple data centers. CRDTs provide a robust way to merge event timelines.
Benefit: Maintains consistent, up-to-date feeds for every user, even under heavy traffic or partial outages.

### IoT and Edge Computing

Example: Smart homes, industrial IoT platforms
Description: Edge devices that intermittently connect to the cloud can store local updates in CRDT form. Once back online, data merges seamlessly into the global state.
Benefit: Guarantees that sensor readings, device settings, or logs remain accurate despite frequent disconnections or network partitions.

### Gaming

Example: Multiplayer online games
Description: CRDTs manage real-time game state (positions, inventories, scores) across distributed servers and clients.
Benefit: Provides a consistent experience with minimal lag-induced conflicts. Local changes get merged as soon as a node reconnects.

How CRDTs Solve Brain-Split Issues

In a typical split-brain scenario, multiple replicas might keep accepting writes, leading to divergent states. CRDT-based data models inherently support:

**Concurrent Updates**  Each partition can make local changes without blocking or waiting on a central coordinator.

**Automated Merging** When partitions rejoin, CRDT merges are commutative and idempotent. No need for manual conflict resolution or overshadowing one partition’s writes.

**High Availability** The system continues to function even if certain regions lose connectivity (e.g., in offline or partitioned mode). Once connectivity is restored, the data converges.


CRDTs are a compelling solution for distributed applications that value availability and concurrency. While they can’t solve every single challenge of distributed systems (e.g., strict global invariants might still require stronger consistency protocols), they significantly reduce the overhead of conflict resolution, especially in split-brain or offline-first scenarios.
Key Takeaways

Resilience Through Concurrency: CRDTs handle parallel updates naturally, reducing friction in collaborative or partition-prone environments.

Split-Brain Savior: With CRDTs, even severe partitions can be tolerated; each side operates independently and reconciles changes automatically.

Broad Use Cases: From chat apps to enterprise-scale e-commerce, CRDTs fit where local writes must remain possible under poor connectivity.

## References 

- [CRDT Resources](https://crdt.tech/resources)
- [Martin Kleppmann's Blog on CRDTs: A comprehensive overview of the state of CRDTs and their applications](https://martin.kleppmann.com/2020/07/06/crdt-hard-parts-hydra.html)
- [Conflict-free Replicated Data Types (CRDTs) in Depth: An in-depth article on how CRDTs work and their use cases in collaborative editing](https://pages.lip6.fr/Marc.Shapiro/papers/RR-7687.pdf)
- [Redis CRDT](https://redis.io/blog/diving-into-crdts/)
- [CRDTs and the Quest for Distributed Consistency](https://www.youtube.com/watch?v=B5NULPSiOGw)
- [Distributed Counter System Design](https://systemdesign.one/distributed-counter-system-design/)
- [CRDT.Tech](https://crdt.tech/)
- [Academic Foundations](https://pages.lip6.fr/Marc.Shapiro/papers/RR-7687.pdf): A Comprehensive Study of Convergent and Commutative Replicated Data Types (Marc Shapiro et al.)  
- [Redis CRDT project (experimental)](https://redis.io/blog/diving-into-crdts/)
- [GunDB for decentralized data storage with CRDT-like sync](https://gun.eco/docs/Conflict-Resolution-with-Guns)
- [Split-Brain Mitigation, RabbitMQ’s discussion on network partitions](https://www.rabbitmq.com/partitions.html)
