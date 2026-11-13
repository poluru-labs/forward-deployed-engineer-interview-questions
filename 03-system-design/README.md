# System design

Design interviews for FDE work: integrations with existing enterprise systems, multi-tenant deployments, data pipelines, and tradeoffs when the customer environment constrains the architecture.

This curated set of 100 interview questions covers system-design fundamentals and practical architecture exercises. “Top 100” means a broad preparation selection, not a frequency ranking. Answers are starting points: clarify the actual requirements, explain alternatives, and adapt the design to the customer. All example volumes and targets are hypothetical.

## Company hashtags and attribution

Every question includes topic and company-status hashtags. **Company relevance does not mean the company asked that question.** Public evidence does not establish an origin for most questions, and no individual interviewer identity is established here.

| Hashtags | Meaning |
| --- | --- |
| `#CompanyUnverified #AmazonRelevant` | Original practice question with unverified interview origin. Relevance is an editorial inference from the broad system-design objectives in [Amazon's official SDE III preparation guide](https://amazon.jobs/content/en-gb/how-we-hire/sde-iii-interview-prep), not attribution of this prompt to Amazon. |
| `#TeamApt #CandidateReported` | Question 91 is a prompt attributed to TeamApt on a public candidate-report page, linked beside the question. |
| `#BlackbirdLabs #CandidateReported` | Question 92 adapts a prompt attributed to Blackbird Labs on a public candidate-report page, linked beside the question. |

Sources reviewed November 12, 2026. There are 98 questions with unverified company origins and two candidate-reported company attributions. Candidate reports are not independent verification and do not establish that a question appeared in an FDE interview. Technical references support concepts, not interview provenance; the answers are original study material, not company-approved solutions.

## Quick cheatsheet

| Area | What to remember |
| --- | --- |
| Interview flow | Clarify → estimate → define APIs and data → sketch → trace a request → examine bottlenecks and failures → summarize tradeoffs. |
| Requirements | Identify users, critical workflows, scale, latency, availability, correctness, security, cost, and ownership. |
| Capacity | Estimate average and peak load, payload sizes, retention, fan-out, and headroom; state assumptions. |
| Starting architecture | Prefer a simple design with clear boundaries; add distribution when requirements justify it. |
| APIs | Define contracts, authorization, validation, pagination, errors, idempotency, and compatibility. |
| Storage | Choose by access patterns and invariants; discuss indexes, transactions, growth, and recovery. |
| Replication vs. partitioning | Replication copies data; partitioning divides it. Neither automatically solves every scaling problem. |
| Consistency | Decide what may be stale and what must be coordinated; explain behavior during failures. |
| CAP | During a partition, a distributed read/write system cannot guarantee both linearizable consistency and availability for every request. |
| Caching | Name the key, owner, invalidation strategy, stale-data tolerance, and failure behavior. |
| Messaging | State delivery, ordering, acknowledgement, replay, deduplication, and poison-message handling. |
| Retries | Set deadlines, retry only suitable failures, bound attempts, add backoff and jitter, and make side effects safe. |
| Reliability | Define SLIs/SLOs, recovery targets, failure domains, observability, rollback, and restore drills. |
| Enterprise delivery | Verify identity, approved data access, network restrictions, tenant isolation, integrations, and support ownership. |
| AI architecture | Separate ingestion and retrieval from inference; evaluate quality, permissions, latency, cost, and tool-action safety. |
| Strong answer | Explain why a component exists, what fails when it breaks, and what evidence would justify changing it. |

**Useful calculations:**

- Average requests/second = requests/day ÷ 86,400. Derive peak load from workload evidence or an explicit assumption.
- Raw storage = records/day × average bytes/record × retention days. Add indexes, replicas, logs, backups, and growth separately.
- Payload bandwidth = requests/second × average payload bytes. Add protocol overhead, replication traffic, and fan-out where applicable.
- Approximate concurrent work = arrival rate × average time in the system, using consistent units and a stable operating window (Little's Law).
- Cache hit ratio = cache hits / eligible lookups. Backend load also depends on misses, refreshes, writes, and retries.
- Time-based error budget = measurement window × (1 − availability target); request-based SLOs use eligible request counts instead.

**Worked estimate:** 8.64 million requests/day averages 100 requests/second. Assuming a tenfold peak gives 1,000 requests/second. At 20 KB per response, peak payload egress is about 20 MB/second using decimal units, before overhead. These estimates guide testing; they do not prove a server's capacity.

## Study map

| Questions | Focus |
| --- | --- |
| 1–10 | Requirements, estimation, and design approach |
| 11–20 | APIs, networking, and service boundaries |
| 21–30 | Databases and storage |
| 31–40 | Distributed systems and correctness |
| 41–50 | Caching and performance |
| 51–60 | Messaging and data pipelines |
| 61–70 | Reliability, observability, and deployment |
| 71–80 | Enterprise architecture and security |
| 81–90 | AI systems and FDE deployment scenarios |
| 91–100 | End-to-end design exercises |

## Questions and detailed answers

### Requirements, estimation, and design approach (1–10)

#### 1. How do you approach a system-design interview?

Tags: #DesignApproach #CompanyUnverified #AmazonRelevant

I first clarify the users, critical workflows, success criteria, and scope. I estimate the workloads that could change architectural decisions, define important APIs and data entities, and propose a simple high-level design. Then I trace a request and examine correctness, bottlenecks, and failure behavior with the interviewer.

I leave time to discuss tradeoffs, operations, and how the design could evolve. For an FDE scenario, I explicitly ask about existing customer systems and deployment restrictions. A strong answer connects every major component to a requirement rather than presenting a memorized collection of technologies.

#### 2. What functional requirements should you clarify first?

Tags: #FunctionalRequirements #CompanyUnverified #AmazonRelevant

I identify the main actors, actions, inputs, outputs, and business rules. I ask which workflows are essential for the initial design and which can be excluded. I also clarify important failure cases, such as duplicate submissions, missing data, and actions that require approval.

For an order system, “place an order” leaves questions about reservations, payment, cancellation, and fulfillment. I choose a bounded scope with the interviewer and define observable outcomes. This prevents spending the entire session designing secondary features while leaving the central workflow ambiguous.

#### 3. How do you define nonfunctional requirements?

Tags: #QualityAttributes #CompanyUnverified #AmazonRelevant

I translate terms such as fast, reliable, and secure into measurable expectations under stated conditions. I ask about latency percentiles, traffic, availability, durability, recovery, growth, and operating constraints. I distinguish essential requirements from preferences and identify which ones conflict.

For example, an interactive lookup and an overnight reconciliation job have different latency needs. Stronger coordination may improve correctness while increasing response time or reducing availability during failures. I make these tensions explicit and confirm priorities before choosing components or promising a particular service level.

#### 4. How do you estimate system capacity?

Tags: #CapacityEstimation #CompanyUnverified #AmazonRelevant

I estimate active users, actions per user, request mix, peak patterns, payload sizes, and retention. I calculate orders of magnitude for request rate, storage, and bandwidth, then identify which resources are likely to constrain the design. I state uncertain assumptions and test sensitivity to them.

For example, fan-out to hundreds of recipients can dominate load even when incoming requests are modest. I include internal amplification and retries rather than sizing only the public API. Final capacity comes from representative benchmarks and failure testing, not arithmetic alone.

#### 5. Why do averages hide important design problems?

Tags: #TailLatency #CompanyUnverified #AmazonRelevant

Average load and latency can conceal bursts, slow tenants, and a small group of very expensive requests. I examine distributions, peak intervals, and relevant latency percentiles. I also distinguish server processing time from end-to-end user experience and queueing delay.

For a request that depends on several services, a slow dependency can dominate completion time. I investigate the actual critical path and correlated failures rather than adding independent percentile figures mechanically. Capacity and timeout decisions should reflect important tail behavior, not only a healthy average.

#### 6. When should you choose a monolith instead of microservices?

Tags: #ServiceArchitecture #CompanyUnverified #AmazonRelevant

I prefer a modular monolith when one team owns the application, requirements are evolving, and independent deployment or scaling is not yet necessary. Clear internal boundaries can preserve maintainability without introducing distributed communication, cross-service consistency, and operational overhead.

Microservices become useful when independently owned domains need isolation, different scaling characteristics, or separate release cycles. I identify those benefits concretely before splitting services. A sensible migration path extracts a well-defined boundary when evidence justifies it, rather than creating a separate network service for every entity at the start.

#### 7. How do you choose between building and using a managed service?

Tags: #BuildVsBuy #CompanyUnverified #AmazonRelevant

I compare required behavior, integration effort, reliability, operating skills, security controls, cost, and portability. A managed service can remove significant operational work, but its quotas, failure modes, data boundaries, and contractual commitments still need evaluation. I include migration and exit costs.

For example, a managed queue may be a good fit unless the customer requires a disconnected deployment or unsupported delivery semantics. I test critical assumptions and calculate total ownership cost. Building a replacement is justified by a real requirement, not by a preference for controlling every component.

#### 8. What makes an architecture diagram useful?

Tags: #ArchitectureCommunication #CompanyUnverified #AmazonRelevant

A useful diagram shows responsibilities, data flow, trust boundaries, state ownership, and relevant deployment boundaries. Arrows should indicate what moves and whether interactions are synchronous or asynchronous. I label authoritative stores separately from caches and derived indexes.

I then trace a concrete request through the diagram, including authentication, authorization, persistence, and failure handling. If I cannot explain why a box exists or who operates it, the design may be underspecified. Additional detail should clarify a decision rather than make the picture look more sophisticated.

#### 9. How do you design for future growth without overengineering?

Tags: #EvolutionaryDesign #CompanyUnverified #AmazonRelevant

I build for the stated workload with measured headroom and identify the next likely bottleneck. I preserve clear interfaces, data ownership, and migration options where they are inexpensive. I avoid introducing complex distributed infrastructure solely because the application might someday become large.

For example, a single relational database with good indexes may meet current needs while supporting later read replicas or partitioning. I describe the metrics and thresholds that would trigger those changes. This gives the interviewer a credible evolution plan without pretending that every growth scenario must be solved immediately.

#### 10. How do you compare two valid architectures?

Tags: #ArchitectureTradeoffs #CompanyUnverified #AmazonRelevant

I compare both against the same requirements: correctness, latency, availability, cost, complexity, security, and maintainability. I identify which assumptions make each option attractive and which risks remain unresolved. I recommend one approach and state what evidence would change the recommendation.

For example, asynchronous processing can reduce user-facing wait time but requires job tracking and recovery. Synchronous processing can be simpler when work is short and dependencies are reliable enough. A good comparison explains the practical consequences for this workflow instead of declaring either pattern universally superior.

### APIs, networking, and service boundaries (11–20)

#### 11. How do you design a clear API contract?

Tags: #APIDesign #CompanyUnverified #AmazonRelevant

I define resources or operations, request and response schemas, authentication, authorization, validation, errors, and compatibility expectations. I include pagination, quotas, concurrency behavior, and idempotency where relevant. The contract should let clients distinguish success, retryable failure, and invalid requests without guessing.

For a long-running import, I might return an accepted job identifier and expose a status endpoint with terminal states. I define what cancellation means and how partial failures are represented. The goal is predictable behavior across both normal execution and uncertain network outcomes.

#### 12. When would you use REST, RPC, or GraphQL?

Tags: #APIStyles #CompanyUnverified #AmazonRelevant

I choose based on clients, operations, tooling, and performance requirements. REST fits resource-oriented interfaces with familiar HTTP behavior. RPC suits explicit operations and strongly defined service contracts. GraphQL can help clients request flexible views but introduces query-cost, authorization, and caching considerations.

These styles can coexist behind clear ownership boundaries. For example, an external resource API might call internal RPC services. I avoid choosing a style purely for fashion and explain how schema evolution, access checks, debugging, and expensive requests will be managed in the selected approach.

#### 13. How should pagination work for a large collection?

Tags: #Pagination #CompanyUnverified #AmazonRelevant

I define a deterministic ordering and choose offset or cursor pagination based on access patterns and mutation behavior. Offset pagination is straightforward but can become expensive or unstable as rows shift. Cursor pagination continues from a known position and typically needs an indexed ordering with a unique tie-breaker.

For example, a cursor might encode a timestamp and record ID without exposing sensitive implementation details. I define whether the client sees a changing collection or a fixed snapshot. Pagination alone does not guarantee a consistent historical view during concurrent inserts, updates, and deletions.

#### 14. What is the difference between a load balancer and an API gateway?

Tags: #TrafficManagement #CompanyUnverified #AmazonRelevant

A load balancer distributes traffic across healthy destinations, using transport- or application-level information depending on the implementation. An API gateway commonly adds API-oriented policies such as authentication, quotas, routing, and request transformations. Some products combine these capabilities, so I describe responsibilities rather than relying on labels.

I keep business authorization in the appropriate service even when the gateway validates identity. I also consider gateway availability, configuration rollout, connection limits, and observability. A centralized policy point should not become an unexplained single point of failure.

#### 15. When would you use polling, server-sent events, or WebSockets?

Tags: #RealtimeCommunication #CompanyUnverified #AmazonRelevant

Polling is simple and appropriate when update frequency and freshness requirements are modest. Server-sent events support server-to-client streaming over HTTP. WebSockets support bidirectional messaging when both sides need to send updates frequently. I consider proxies, connection lifetime, authentication, reconnect behavior, and operational capacity.

For progress updates, server-sent events may be enough; collaborative editing may require bidirectional communication. With any approach, I design sequence identifiers or resume tokens where needed so reconnecting does not silently lose updates. A live connection is not itself a durable message history.

#### 16. How do DNS, TLS, and connection setup affect performance?

Tags: #NetworkLatency #CompanyUnverified #AmazonRelevant

User-visible latency can include name resolution, connection establishment, TLS negotiation, network transit, and application processing. I measure those phases before assuming the server code is slow. Connection reuse, geographic placement, and appropriate caching can reduce repeated setup costs.

I also account for certificate renewal, trusted endpoints, and the customer's proxy configuration. An integration can work in one environment and fail in another because of trust stores or routing restrictions. I preserve secure verification and fix the configuration rather than disabling certificate checks to make a connection succeed.

#### 17. How do you choose timeouts and propagate deadlines?

Tags: #Timeouts #CompanyUnverified #AmazonRelevant

I start with the caller's total latency budget and downstream latency measurements. Each hop receives enough time to do useful work without allowing dependencies to hold resources indefinitely. I reserve time for response handling and any permitted retry, and propagate cancellation where supported.

A timeout does not prove the operation had no effect. I define how clients query or safely retry an uncertain result. Excessively short timeouts can create unnecessary retries; excessively long ones can exhaust connections and workers. See the [AWS guidance on timeouts and retries](https://aws.amazon.com/builders-library/timeouts-retries-and-backoff-with-jitter/).

#### 18. How do you evolve an API without breaking clients?

Tags: #APICompatibility #CompanyUnverified #AmazonRelevant

I define compatibility rules and prefer additive changes that existing clients can tolerate. I avoid changing field meaning, required inputs, or error behavior without coordination. For breaking changes, I provide a migration path, measure remaining old-client usage, and agree on deprecation timing.

For example, adding an optional field can still break a strict consumer or a closed enum handler. I validate real client behavior with contract checks and staged rollout. Version numbers help communicate changes, but they do not replace a deployment and support plan for affected customers.

#### 19. How do you design an idempotent create operation?

Tags: #IdempotentAPIs #CompanyUnverified #AmazonRelevant

I scope an idempotency key to the caller and operation, associate it with a request fingerprint, and persist the result atomically with the authoritative state change where possible. Matching retries return the previous result; conflicting payloads are rejected. I define retention and concurrent-request behavior.

For an external side effect, I need a compatible downstream idempotency mechanism or reconciliation process. A separate “check then create” sequence can race. I also explain what happens when the key expires, because deduplication over a limited window is not a permanent guarantee against duplicates.

#### 20. How do you control expensive or abusive API traffic?

Tags: #AdmissionControl #CompanyUnverified #AmazonRelevant

I combine identity-aware quotas, request-size limits, concurrency limits, and operation-specific budgets. I validate before performing expensive work and distinguish expected bursts from sustained overload. Limits should protect shared resources while providing actionable feedback to legitimate clients.

For example, a large export may consume far more capacity than a metadata lookup, so one request counter may be insufficient. I consider weighted cost, asynchronous execution, and tenant fairness. I also define behavior when the limiting infrastructure fails instead of leaving availability and protection tradeoffs implicit.

### Databases and storage (21–30)

#### 21. How do you choose a database for a system?

Tags: #DatabaseSelection #CompanyUnverified #AmazonRelevant

I start with access patterns, relationships, invariants, write volume, query flexibility, and operating constraints. A relational database is often a strong starting point when transactions and joins matter. Other models can help when document access, key-based scale, graph traversal, or specialized search dominates.

I compare concrete capabilities instead of assuming SQL cannot scale or NoSQL has no transactions. I also consider backup, migration, tooling, and team expertise. The choice should support the actual workload and consistency needs, with a plan for the bottlenecks expected at the stated scale.

#### 22. How do you design a data model from requirements?

Tags: #DataModeling #CompanyUnverified #AmazonRelevant

I identify entities, identifiers, relationships, lifecycle states, and business invariants. I map the important reads and writes to the model and decide which data is authoritative versus derived. I include tenant ownership, timestamps, and deletion behavior where relevant.

For an order workflow, I distinguish an order, its line items, reservations, and payment attempts. Combining everything into one mutable status can hide important transitions and failures. I validate the model using concrete scenarios such as cancellation after payment authorization or a retry following a lost response.

#### 23. When should you normalize or denormalize data?

Tags: #Normalization #CompanyUnverified #AmazonRelevant

Normalization reduces duplicated facts and helps maintain consistent updates. Denormalization can improve a specific read path by storing duplicated or precomputed information. I choose based on observed access patterns and explain how duplicated data will be maintained and repaired.

For example, a read model may store a customer's display name beside an order summary while the customer record remains authoritative. I define whether historical orders should show the current name or the name at purchase. Denormalization is a correctness and update-policy decision as well as a performance optimization.

#### 24. How do indexes improve performance, and what do they cost?

Tags: #Indexing #CompanyUnverified #AmazonRelevant

Indexes provide alternative access paths that can reduce scanned data for suitable queries. I choose indexes using filters, ordering, join patterns, and actual query plans. Composite column order and selectivity matter, and an index is not guaranteed to help every query.

Indexes consume storage and add work to writes and maintenance. I validate improvements against representative data rather than adding an index for every field. For a tenant-scoped recent-record query, an index aligned with tenant and ordering can help, but it does not replace authorization or eliminate all possible hot-tenant issues.

#### 25. What does transaction isolation protect against?

Tags: #TransactionIsolation #CompanyUnverified #AmazonRelevant

Isolation controls how concurrent transactions observe and affect one another. I identify the business invariant and choose locking, conditional updates, constraints, or an appropriate isolation level to preserve it. Transaction boundaries alone do not imply that every read-modify-write sequence is safe.

Serializable execution can protect broader invariants but may abort conflicting transactions, so the application needs an appropriate retry path. Exact behavior varies by database; I verify the implementation rather than relying on level names alone. See [PostgreSQL's isolation documentation](https://www.postgresql.org/docs/current/transaction-iso.html).

#### 26. What is the difference between replication and sharding?

Tags: #ReplicationAndSharding #CompanyUnverified #AmazonRelevant

Replication maintains copies of data for availability, durability, or read distribution. Sharding divides data across partitions to distribute storage or workload. A shard can itself have replicas, and neither technique removes the need for sound query design and failure handling.

I explain write ownership, replication lag, shard routing, and rebalancing. For example, read replicas can serve stale results if replication is asynchronous. Sharding by tenant can simplify routing but make one large tenant a hotspot. The design must address these consequences rather than treating additional database nodes as automatic scalability.

#### 27. How do you choose a partition key?

Tags: #PartitionKeys #CompanyUnverified #AmazonRelevant

I balance workload distribution, query locality, key cardinality, and future rebalancing. A useful key spreads load while allowing important requests to reach a small number of partitions. I examine skew and hot keys rather than assuming all tenants or entities have similar traffic.

For example, time-based keys can concentrate current writes on one partition, while random distribution can make range queries expensive. I may combine hashing with a locality boundary or split unusually large tenants. I explain routing changes and data movement so growth does not depend on a disruptive manual migration.

#### 28. How do you store large files or media?

Tags: #ObjectStorage #CompanyUnverified #AmazonRelevant

I generally separate large binary content from queryable metadata. Object storage holds the file, while a database tracks ownership, state, version, checksum, and access information. I use authorized upload and download flows and define size limits, lifecycle rules, and cleanup of abandoned uploads.

The metadata and object write are separate operations, so I design intermediate states and reconciliation. For example, a record can remain pending until upload verification succeeds. I avoid assuming that a successful metadata transaction proves the file exists or that an unguessable object URL provides adequate authorization.

#### 29. How do you design search without making the search index authoritative?

Tags: #SearchArchitecture #CompanyUnverified #AmazonRelevant

I keep canonical records in an authoritative store and build a derived index for search-specific access patterns. An ingestion process applies creates, updates, and deletions, with versioning or ordering controls to avoid stale events overwriting newer state. I monitor indexing lag and rejected documents.

I define acceptable freshness and verify permissions when serving results and fetching content. Rebuilding the index from a trusted source should be possible. If deleted or restricted content remains in a stale index, the read path must still enforce access requirements rather than trusting the index to be perfectly current.

#### 30. How do you change a database schema safely?

Tags: #SchemaMigration #CompanyUnverified #AmazonRelevant

I use compatible stages: introduce the new representation, deploy code that tolerates both forms, backfill carefully, switch reads or writes, and remove the old form after verification. I assess locks, transaction size, replication effects, and the time needed for large data changes.

For example, renaming a field immediately can break old application instances during a rolling deployment. An expand-and-contract approach allows mixed versions temporarily. I define reconciliation and recovery steps because reverting application code may not reverse a completed data transformation or an irreversible deletion.

### Distributed systems and correctness (31–40)

#### 31. What does the CAP theorem mean in practice?

Tags: #CAPTheorem #CompanyUnverified #AmazonRelevant

During a network partition, a distributed read/write system cannot guarantee both linearizable consistency and availability for every request. One side may need to reject or delay operations to avoid conflicting results, or the system may accept operations that later require reconciliation.

This is not a permanent instruction to pick any two letters, and CAP availability differs from an uptime percentage. I discuss the decision for specific operations. A public catalog may tolerate stale reads, while a limited-inventory reservation may need coordination and refuse writes when authoritative state is unreachable.

#### 32. What is the difference between strong and eventual consistency?

Tags: #ConsistencyModels #CompanyUnverified #AmazonRelevant

I clarify the exact guarantees instead of using “strong” loosely. Linearizable operations appear to take effect atomically in an order consistent with real time. Eventual consistency allows replicas to differ temporarily while expecting convergence under the model's conditions once updates stop.

I connect the choice to user behavior and invariants. A delayed profile update may be acceptable, while a stale permission decision may be unsafe. Eventual consistency by itself does not specify a maximum staleness bound, session guarantees, or conflict policy; those need to be designed and communicated separately.

#### 33. When do you need distributed consensus?

Tags: #Consensus #CompanyUnverified #AmazonRelevant

Consensus is useful when participating nodes must agree on an ordered state or authoritative decision despite certain failures. Examples include coordinating replicated metadata or selecting a leader under a protocol's assumptions. It introduces communication cost and requires enough participating nodes to make progress.

I prefer established coordination systems or database guarantees over inventing a protocol. I state the failure model, quorum requirements, and behavior when a quorum is unavailable. Consensus does not automatically coordinate external side effects, and adding it everywhere can increase latency and reduce availability without solving a real requirement.

#### 34. What is split-brain, and how do you prevent conflicting leaders?

Tags: #LeaderElection #CompanyUnverified #AmazonRelevant

Split-brain occurs when multiple nodes act as authoritative leaders for the same responsibility. I use a coordination mechanism that constrains authority and ensure stale leaders cannot continue making accepted writes. The resource receiving writes must participate in enforcing that authority.

For example, monotonically increasing fencing tokens can let a storage service reject work from an older leader. A lease timeout alone is insufficient if an old process pauses and later resumes. I explain failover, renewal, and enforcement under delayed messages rather than assuming election immediately stops the previous leader.

#### 35. What does exactly-once processing really mean?

Tags: #DeliverySemantics #CompanyUnverified #AmazonRelevant

I ask which effect and boundary the guarantee covers. A platform may atomically coordinate consumed offsets and produced records within its own transaction model, while an external email or payment still needs separate protection. At-least-once delivery with idempotent effects is often a practical end-to-end design.

I describe acknowledgements, atomic state changes, deduplication, and recovery after a crash at each boundary. “Exactly once” should not hide uncertainty about external side effects or expiring deduplication records. See [Kafka's discussion of delivery semantics](https://kafka.apache.org/41/design/design/#message-delivery-semantics).

#### 36. How do you coordinate a workflow across several services?

Tags: #DistributedWorkflows #CompanyUnverified #AmazonRelevant

I identify each step, its authoritative state, and the invariant that must hold across services. Where a single transaction is unavailable, I may use a saga with explicit progress, retries, and compensating actions. I compare orchestration with event-based coordination based on visibility and coupling.

Compensation is a new business action, not a perfect rewind. Releasing a reservation may be straightforward, while undoing a shipment may be impossible. I model irreversible steps carefully and include manual intervention states so failures do not disappear into an endless retry loop.

#### 37. How does the transactional outbox pattern work?

Tags: #TransactionalOutbox #CompanyUnverified #AmazonRelevant

The application writes its business change and an outbox record in the same local database transaction. A separate publisher reads committed outbox records and delivers them to a broker or destination. This removes the gap where the database commits but the application crashes before recording an event to publish.

The publisher can still deliver duplicates if it crashes after delivery but before marking completion. Consumers therefore need appropriate deduplication or idempotent effects. I also design ordering, cleanup, monitoring, and recovery from a backlog; the outbox is a reliable handoff mechanism, not a universal exactly-once guarantee.

#### 38. How do you handle conflicting concurrent updates?

Tags: #ConcurrencyControl #CompanyUnverified #AmazonRelevant

I choose a policy based on the data's meaning. Options include locking, conditional writes using a version, domain-specific merging, or asking the user to resolve a conflict. Last-write-wins can be acceptable for some preferences but can silently discard important edits.

For an approval state, I might require that the stored version and current state match the expected values before accepting a transition. A conflict returns a clear result and triggers a refresh or retry. I avoid assuming that wall-clock timestamps reliably establish the correct business order across machines.

#### 39. What challenges do distributed clocks create?

Tags: #TimeAndOrdering #CompanyUnverified #AmazonRelevant

Machine clocks can differ and can change because of synchronization or administrative adjustments. I avoid using wall-clock time alone to establish global ordering, detect ownership, or measure elapsed durations. I use suitable monotonic clocks for local durations and explicit versions or sequence numbers for ordering where needed.

Timestamps remain useful for observation and approximate event time, but I document their limitations. For example, a late-arriving event may have an earlier timestamp than already processed data. The design needs a policy for late events rather than assuming timestamp sorting guarantees correct execution.

#### 40. How do you prevent overselling the last available item?

Tags: #InventoryCorrectness #CompanyUnverified #AmazonRelevant

I enforce the stock invariant at the authoritative write boundary using an atomic conditional decrement, suitable transaction, or equivalent serialized operation. A separate availability check followed by an unprotected update can race. I distinguish reservation from final purchase and define expiry, cancellation, and retry behavior.

Caches and asynchronous replicas can display approximate availability but should not authorize the final reservation. If payment outcome is uncertain, I reconcile before releasing or confirming stock. I test simultaneous requests and crash boundaries, including recovery after the reservation succeeds but the client loses the response.

### Caching and performance (41–50)

#### 41. When should you add a cache?

Tags: #Caching #CompanyUnverified #AmazonRelevant

I add a cache when measurements show repeated expensive work and the workload has enough reuse to benefit. I define what is cached, the key, memory budget, freshness requirements, and behavior on a miss or failure. I estimate whether the cache will actually reduce the constrained resource.

For example, caching common reference data may reduce database load, while caching mostly unique requests may provide little benefit. I treat the cache as an optimization unless explicitly designed otherwise. The backend still needs a protection strategy when the cache is cold or unavailable.

#### 42. What are cache-aside, write-through, and write-behind?

Tags: #CachePatterns #CompanyUnverified #AmazonRelevant

With cache-aside, the application loads from the source on a miss and populates the cache. Write-through routes writes through a caching layer that also updates durable storage. Write-behind acknowledges before asynchronous persistence and therefore needs explicit durability and recovery guarantees.

I choose based on read/write patterns, failure tolerance, and implementation semantics. None of these names resolves concurrent updates or partial failures automatically. For customer records, I would explain which store is authoritative and how the system avoids serving an older value after a newer write, including races during cache population.

#### 43. How do you invalidate cached data correctly?

Tags: #CacheInvalidation #CompanyUnverified #AmazonRelevant

I connect invalidation to the source of updates and the acceptable staleness window. Options include expiration, explicit invalidation, versioned keys, and event-driven refresh. I examine races between readers, writers, invalidation delivery, and delayed refreshes rather than assuming deletion after a write guarantees freshness.

For example, an in-flight reader can repopulate stale data after invalidation. Versions or conditional updates may prevent older values from replacing newer ones. For permission-sensitive content, I may recheck authority at read time because a missed invalidation must not silently preserve access that was revoked.

#### 44. How do you prevent a cache stampede?

Tags: #CacheStampede #CompanyUnverified #AmazonRelevant

A stampede occurs when many misses trigger the same expensive backend work simultaneously. I can coalesce refreshes, use bounded per-key coordination, refresh ahead, or serve stale data while refreshing where the workflow permits. Jittered expirations help avoid many unrelated keys expiring together.

I also cap backend concurrency and define what happens if the refresh owner fails. Stale data is acceptable only within the agreed correctness constraints. For example, an old public description may be fine temporarily, while a revoked permission or final inventory decision needs a different path.

#### 45. How do you handle hot keys and uneven traffic?

Tags: #HotKeys #CompanyUnverified #AmazonRelevant

I identify whether the hotspot comes from one popular item, one large tenant, or a skewed partitioning strategy. Responses include caching replicated reads, request coalescing, tenant isolation, and partition splitting. The appropriate option depends on whether reads, writes, or coordination are the bottleneck.

A read hotspot may be distributed across caches, but a globally ordered counter still requires a defined write-coordination strategy. I evaluate whether the requirement can tolerate aggregation or approximation before distributing updates. Monitoring should expose per-key or per-tenant skew without creating unbounded metrics cardinality.

#### 46. What should be cached at a CDN?

Tags: #CDN #CompanyUnverified #AmazonRelevant

A CDN can serve cacheable content near users and reduce origin traffic. I classify public static assets, public dynamic responses, and private user-specific data separately. I define cache keys, expiration, variation, invalidation, and access behavior with particular care for authenticated responses.

For versioned assets, content-based URLs can simplify long-lived caching. For personalized content, a mistaken shared cache key can expose one user's data to another. I also plan origin protection and cache-miss behavior; a globally distributed cache does not eliminate the need to withstand refresh traffic or enforce authorization.

#### 47. How do you investigate a slow endpoint?

Tags: #PerformanceDiagnosis #CompanyUnverified #AmazonRelevant

I establish the affected workflow, time window, workload, and expected latency. I use traces, query plans, and resource metrics to separate network, queueing, application, database, and downstream time. I compare fast and slow requests and check recent changes before choosing an optimization.

If a query dominates, an index or reduced data retrieval may help; if connections are exhausted, adding application replicas could worsen the problem. I validate the fix under comparable load and inspect error rate and tail latency. A lower average alone does not prove the user-facing problem is resolved.

#### 48. How do you design backpressure?

Tags: #Backpressure #CompanyUnverified #AmazonRelevant

Backpressure limits incoming work when downstream processing cannot keep up. I use bounded queues, concurrency controls, explicit admission decisions, or producer slowdown depending on the protocol. I define which work can wait, be rejected, be dropped, or be degraded.

An unbounded queue converts overload into memory exhaustion or unacceptable delay. For example, a report service can return a clear capacity response or accept only bounded scheduled work. I monitor queue age and completion time as well as depth so the system protects user deadlines instead of merely storing an ever-growing backlog.

#### 49. How should autoscaling decisions be made?

Tags: #Autoscaling #CompanyUnverified #AmazonRelevant

I scale using a signal related to the actual bottleneck, such as utilization, outstanding work, or queue age. I account for startup time, minimum capacity, cooldown behavior, quotas, and cost limits. I also verify that downstream systems can handle the additional concurrency.

For queue workers, more replicas help only until a shared database or external quota becomes the constraint. I keep admission controls and headroom for sudden bursts because scaling is not instantaneous. Load tests should include scale-up, scale-down, and failure conditions rather than measuring only steady-state throughput.

#### 50. How do you control infrastructure cost without harming the outcome?

Tags: #CostEngineering #CompanyUnverified #AmazonRelevant

I estimate cost per meaningful unit, such as a successful task, tenant, or processed document. I inspect compute, storage, replication, egress, managed-service charges, and operational effort. I optimize the dominant costs while preserving the agreed quality and reliability requirements.

For example, shorter retention or asynchronous batch processing may reduce cost when the workflow allows it. Aggressive caching may shift cost rather than eliminate it, and a cheaper component may require more support labor. I validate changes with workload evidence and make cost-versus-service-level tradeoffs explicit to the owner.

### Messaging and data pipelines (51–60)

#### 51. When should you use a queue instead of a synchronous call?

Tags: #AsynchronousProcessing #CompanyUnverified #AmazonRelevant

I use a queue when work can complete later, bursts need buffering, or processing needs an independent retry and scaling lifecycle. I preserve synchronous calls when an immediate result is necessary and the operation can meet the latency budget. The business workflow determines which behavior is acceptable.

A queued operation needs durable acceptance, a job identifier, progress or status semantics, and recovery behavior. I distinguish “accepted” from “completed” in the API. A queue does not remove failure; it moves some failure handling and user communication into the asynchronous workflow.

#### 52. What is the difference between a work queue and publish-subscribe?

Tags: #MessagingPatterns #CompanyUnverified #AmazonRelevant

A work queue typically distributes tasks among competing workers so one worker handles each delivery attempt. Publish-subscribe distributes events to independent subscribers, each maintaining its own processing progress according to the platform. A retained log can support replay and multiple consumer groups.

I state the actual broker's semantics instead of assuming the pattern name guarantees durability or ordering. For example, billing and analytics may both consume an order event, while multiple thumbnail workers share one task stream. Each consumer needs its own failure handling and capacity plan so one slow subscriber does not block unrelated responsibilities.

#### 53. How do you make a message consumer safe under redelivery?

Tags: #ConsumerIdempotency #CompanyUnverified #AmazonRelevant

I identify a stable event identifier and make the intended effect idempotent or deduplicate it within the relevant durability boundary. Where possible, I commit the business effect and processed-event record atomically. I acknowledge only after the required state is durable.

If the process crashes after committing but before acknowledgement, the next delivery should recognize the completed effect. For external actions, I need downstream idempotency or reconciliation. I define deduplication retention and replay expectations because deleting the tracking record too early can make an old event produce the effect again.

#### 54. How do you preserve event ordering while scaling consumers?

Tags: #EventOrdering #CompanyUnverified #AmazonRelevant

I clarify whether ordering is required globally or only per entity. Partitioning by an entity key can keep its events in one ordered stream while allowing different entities to process in parallel. Consumer execution must also preserve that order; parallel processing can undo the guarantee after delivery.

I handle missing versions, duplicates, and late events explicitly. A single global order can become a bottleneck, so I require a business justification for it. Partition-level ordering and consumer-group behavior are discussed in the [Kafka design documentation](https://kafka.apache.org/41/design/design/).

#### 55. How do you manage retries and poison messages in a pipeline?

Tags: #PoisonMessages #CompanyUnverified #AmazonRelevant

I classify transient dependency failures separately from malformed or permanently unsupported messages. I use bounded retries with appropriate delay and move exhausted work to an inspectable failure state or dead-letter queue. Alerts, ownership, and a controlled replay process make failures actionable.

For ordered streams, skipping one event can invalidate later processing, so I define whether to block that entity, repair the event, or use a domain-specific fallback. A dead-letter queue is not a resolution by itself. Replaying corrected work must also preserve permissions, idempotency, and the intended ordering.

#### 56. When do you choose batch processing versus streaming?

Tags: #BatchVsStreaming #CompanyUnverified #AmazonRelevant

I choose based on how quickly new information must influence a decision, along with volume, cost, and recovery needs. Batch processing often simplifies operations when periodic results are sufficient. Streaming is useful when low data latency provides meaningful value and justifies continuous processing complexity.

For example, daily reconciliation may work well as a batch, while operational anomaly detection may need faster updates. I distinguish event time from processing time and design for late data if streaming is chosen. Both approaches need validation, replay, and clear treatment of partial or invalid input.

#### 57. How do you design change data capture from a customer database?

Tags: #ChangeDataCapture #CompanyUnverified #AmazonRelevant

I confirm approved access, source support, schema ownership, and operational impact before choosing log-based capture, events, or polling. I coordinate an initial snapshot with a change-stream position so updates during the snapshot are not missed. I preserve enough metadata to apply changes correctly.

I handle deletes, schema evolution, lag, and interrupted capture, including source-log retention limits. If the connector falls behind beyond recoverable history, I need a resnapshot plan. I avoid treating raw database changes as complete business events when consumers need domain meaning that is not present in individual row updates.

#### 58. How do you design a replayable data pipeline?

Tags: #Replay #CompanyUnverified #AmazonRelevant

I retain approved source data or events with stable identifiers, versions, and lineage sufficient to reproduce derived state. I separate transformation logic from irreversible external actions and track processing progress. Replays should target an isolated destination or use explicitly safe update semantics.

For example, rebuilding a search index can process old events without resending historical customer notifications. I define retention, code-version compatibility, and how to handle data that must be deleted. A replay capability must respect current authorization and deletion requirements, not automatically resurrect information that is no longer permitted.

#### 59. How do you handle schema evolution in event streams?

Tags: #EventSchemas #CompanyUnverified #AmazonRelevant

I define an event contract and compatibility rules, including how consumers handle unknown fields, missing values, and new event types. I coordinate producer and consumer changes and use validation or a schema registry where it supports the chosen format and workflow. Breaking changes need a staged migration.

For example, changing an amount's unit without changing its representation can silently corrupt downstream calculations. A new field or version with explicit meaning is safer. I also consider replay: old records must remain interpretable by the processing code or a documented transformation path.

#### 60. How do you detect silent data loss or corruption?

Tags: #DataReconciliation #CompanyUnverified #AmazonRelevant

I combine transport metrics with business-level checks such as counts, totals, identifier coverage, and domain invariants over defined windows. I track provenance and rejected records so missing output can be traced to a source or processing decision. Successful job completion alone does not establish correct data.

For example, matching total record counts can still hide duplicated and missing records that cancel out numerically. I reconcile stable identifiers and relevant values, accounting for expected lag and transformations. Repair uses a controlled, auditable process with validation rather than blindly copying one system over another.

### Reliability, observability, and deployment (61–70)

#### 61. How do you define SLIs, SLOs, and SLAs?

Tags: #ServiceObjectives #CompanyUnverified #AmazonRelevant

An SLI measures a service behavior, such as successful requests or completion latency. An SLO sets a target for that indicator over a defined population and window. An SLA is an agreement that may specify commitments and consequences; it should not be inferred from an internal target.

I choose indicators that reflect user outcomes and define valid requests and exclusions explicitly. A healthy API response rate can coexist with failed background jobs. I would measure those workflows separately. See [Google's SRE chapter on service-level objectives](https://sre.google/sre-book/service-level-objectives/).

#### 62. How do you avoid single points of failure?

Tags: #FailureDomains #CompanyUnverified #AmazonRelevant

I examine the entire request and recovery path for components whose failure stops the service. I distribute redundancy across appropriate failure domains and verify that routing, capacity, configuration, and data remain available after a loss. Two instances sharing the same critical dependency may still fail together.

I also inspect deployment systems, credentials, DNS, and operational access. Failover needs enough surviving capacity and a tested authority model for stateful components. I validate the design with failure exercises rather than counting replicas and assuming independence from the number of boxes in the diagram.

#### 63. What are circuit breakers, bulkheads, and load shedding?

Tags: #ResiliencePatterns #CompanyUnverified #AmazonRelevant

A circuit breaker limits calls to a dependency that appears unhealthy and later probes for recovery. Bulkheads isolate resource pools so one workload cannot exhaust everything. Load shedding rejects or degrades work when the system lacks safe capacity. These patterns address different failure mechanisms.

For example, separate worker pools can protect interactive requests from large exports, while a breaker avoids repeatedly waiting on a failing provider. I define thresholds, recovery behavior, and user-visible results. These controls need coordination with retries and timeouts so their interactions do not create oscillation or amplify failures.

#### 64. How do you prevent retry storms?

Tags: #RetryBudgets #CompanyUnverified #AmazonRelevant

I retry only suitable transient failures and only when the operation is safe to repeat. I use bounded attempts, exponential backoff, jitter, and a total deadline. I limit retry amplification across layers so a small number of client requests cannot generate a large downstream burst.

I also respect overload signals and use admission controls or retry budgets. During a dependency outage, more retries may worsen recovery rather than improve success. The [AWS Builders' Library](https://aws.amazon.com/builders-library/timeouts-retries-and-backoff-with-jitter/) explains how backoff and jitter help prevent synchronized retry traffic.

#### 65. What observability would you include from the start?

Tags: #Observability #CompanyUnverified #AmazonRelevant

I include metrics for traffic, errors, latency, resource saturation, and business task completion. Structured logs and distributed traces connect a request or job across components using correlation identifiers. I track dependency behavior and queue age for asynchronous work.

I control sensitive data exposure, retention, sampling, and label cardinality. Alerts should correspond to actionable impact and identify an owner and runbook. For example, a growing job backlog matters more when linked to completion deadlines than when expressed only as an arbitrary queue-depth threshold with no workflow context.

#### 66. How do you design disaster recovery?

Tags: #DisasterRecovery #CompanyUnverified #AmazonRelevant

I agree on a recovery time objective and recovery point objective for each critical data set and workflow. I design backups, replication, infrastructure restoration, and dependency access to meet those targets. Recovery requires usable credentials, keys, configuration, and people, not only a database snapshot.

I test restore procedures and validate business correctness after recovery. Replication alone is insufficient protection against accidental deletion or corruption that spreads to replicas. I document actual measured restore time and data loss exposure, and I update the plan when the system's size or dependencies change.

#### 67. How do you roll out a change safely?

Tags: #SafeDeployment #CompanyUnverified #AmazonRelevant

I validate compatibility, automate repeatable deployment steps, and expose a limited portion of traffic or users first when appropriate. I compare health and business indicators with clear stop conditions. Feature controls can separate deployment from activation if their behavior and ownership are well defined.

I prepare rollback or roll-forward steps, including database and external side effects. A code rollback cannot always undo transformed data or sent messages. I also consider mixed-version behavior during rollout and verify that the team can disable the change through an available, tested operational path.

#### 68. When does a multi-region architecture make sense?

Tags: #MultiRegion #CompanyUnverified #AmazonRelevant

I use multiple regions when geographic latency, failure isolation, data placement, or recovery requirements justify the additional cost and complexity. I define write ownership, replication behavior, routing, and what happens during a region loss or network partition. Active-active and active-passive designs have different operational demands.

For example, routing users to a healthy region is insufficient if their data or identity dependency remains unavailable. I test capacity, failover, failback, and conflict handling. I avoid assuming that multi-region deployment automatically provides zero downtime or zero data loss.

#### 69. How would you load-test a proposed architecture?

Tags: #LoadTesting #CompanyUnverified #AmazonRelevant

I create a workload matching important request types, data sizes, user distributions, and burst patterns. I test steady load, spikes, sustained operation, and selected failures. I measure end-to-end latency, errors, saturation, queueing, and recovery, while ensuring the test generator is not the bottleneck.

Warm-cache success alone is insufficient if cold starts or cache loss are plausible. I inspect database growth and downstream quotas as well. The result should identify safe operating limits and degradation behavior, not merely the maximum throughput achieved during one favorable run.

#### 70. What makes a system operationally maintainable?

Tags: #Operability #CompanyUnverified #AmazonRelevant

A maintainable system has clear ownership, understandable components, repeatable changes, useful diagnostics, and tested recovery procedures. I include runbooks, dependency inventories, access management, and a support model appropriate to the customer's team. I reduce manual steps that are easy to forget or perform incorrectly.

Before handoff, I ask someone outside the implementation team to perform a routine operation and diagnose a simulated failure using the documentation. This tests whether knowledge has actually transferred. A successful demo does not establish that the receiving team can safely support the service.

### Enterprise architecture and security (71–80)

#### 71. How do you design a multi-tenant application?

Tags: #MultiTenancy #CompanyUnverified #AmazonRelevant

I define the tenant boundary, identity mapping, data ownership, isolation requirements, and workload variation. Shared infrastructure can reduce cost, while dedicated resources can improve isolation or accommodate special constraints. A hybrid approach may place unusually large or restricted tenants separately.

Tenant context must flow through queries, caches, jobs, files, and observability without trusting an arbitrary client-supplied identifier. I test cross-tenant denial paths and resource fairness. See [Microsoft's multitenant architecture guidance](https://learn.microsoft.com/en-us/azure/architecture/guide/multitenant/overview) for the distinction between tenants and individual users and the breadth of architectural considerations.

#### 72. How do you separate authentication from authorization?

Tags: #IdentityAndAccess #CompanyUnverified #AmazonRelevant

Authentication establishes the identity behind a request; authorization determines whether that identity may perform an action on a particular resource. I validate credentials and then apply action-, resource-, and tenant-specific checks at trusted enforcement points. A valid session does not grant access to every record.

I use deny-by-default behavior and test both allowed and denied operations, including background and administrative paths. I account for revocation and policy changes. The [OWASP authorization guidance](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html) emphasizes least privilege and permission checks on every request.

#### 73. How do you integrate enterprise SSO and user provisioning?

Tags: #EnterpriseIdentity #CompanyUnverified #AmazonRelevant

I clarify the customer's identity provider, approved federation protocol, tenant mapping, and account lifecycle. I distinguish signing in from provisioning, group synchronization, and deprovisioning. I validate tokens or assertions using the protocol's required issuer, audience, signature, and replay protections as applicable.

I map external identities to stable internal records without assuming email is a permanent unique identifier. I also define how role changes and account removal affect existing sessions and queued work. A successful login demo should not be mistaken for a complete enterprise access-management design.

#### 74. How should services handle secrets and encryption keys?

Tags: #SecretsManagement #CompanyUnverified #AmazonRelevant

I use an approved secret or key-management mechanism with scoped access, auditability, rotation, and revocation. I separate environments and avoid embedding credentials in source code, images, client bundles, or logs. Where available and appropriate, workload identity can reduce reliance on long-lived shared credentials.

I include renewal and outage behavior in the design. For example, rotating a database credential requires a compatible rollout and connection-management plan. Encrypting data also requires protecting and recovering the keys; a backup that cannot be decrypted during an incident does not meet the recovery requirement.

#### 75. How do you design audit logging?

Tags: #AuditLogs #CompanyUnverified #AmazonRelevant

I identify which actions require an audit trail and record actor, action, target, outcome, time, and relevant request context. I protect the logs from unauthorized alteration and define access, retention, and investigation workflows. I minimize sensitive payloads instead of copying entire requests indiscriminately.

I decide how critical actions behave if audit capture is unavailable. For high-impact changes, durable audit recording may need to be coupled to the action or a reliable handoff. Ordinary debug logs alone may be incomplete, sampled, or mutable and therefore unsuitable as the authoritative audit record.

#### 76. How do you design for data retention and deletion?

Tags: #DataLifecycle #CompanyUnverified #AmazonRelevant

I confirm the customer's approved lifecycle requirements with responsible owners and map where information exists: source stores, replicas, indexes, caches, exports, logs, and backups. I distinguish logical removal from eventual physical cleanup and define how completion is tracked.

Deletion needs to propagate to derived systems and remain effective during replay or restore. For example, a restored backup may require applying a current deletion ledger before serving data. I document technical limits and review obligations with the appropriate specialists instead of promising that one database delete instantly removes every copy.

#### 77. How do you integrate with a legacy system that has no modern API?

Tags: #LegacyIntegration #CompanyUnverified #AmazonRelevant

I investigate approved options such as scheduled exports, supported database views, file exchange, or an existing integration gateway. I work with the system owner to understand load limits, change processes, and support responsibilities. I avoid unsupported direct writes that bypass business rules.

I put an adapter around the legacy interface and define validation, reconciliation, and failure behavior. A batch integration may be the simplest reliable approach if the workflow tolerates delay. I make freshness visible so users do not interpret yesterday's snapshot as a live authoritative view.

#### 78. How do you deploy into a restricted customer network?

Tags: #RestrictedNetworks #CompanyUnverified #AmazonRelevant

I confirm ingress and egress rules, proxy behavior, DNS, trust stores, identity access, and approved deployment channels early. I list external dependencies and determine whether each has an allowed route or a supported local alternative. I validate connectivity from the actual target environment.

I package artifacts reproducibly and plan updates, monitoring, and support access within the customer's controls. If a dependency is unavailable, I adjust the architecture or scope. I do not make bypassing network policy a prerequisite for a pilot that is supposed to represent a viable production deployment.

#### 79. How do you prevent one tenant from exhausting shared resources?

Tags: #NoisyNeighbor #CompanyUnverified #AmazonRelevant

I apply tenant-aware quotas, concurrency budgets, queue fairness, and resource limits at relevant boundaries. I measure consumption by workload and isolate unusually demanding tasks or tenants where necessary. A single global rate limit cannot ensure fairness across customers with different traffic patterns.

For example, bulk exports can use separate worker capacity from interactive lookups. I also examine database hotspots and storage growth, not just HTTP traffic. The design should explain how limits are configured, how customers receive feedback, and what happens when a tenant legitimately needs a larger allocation.

#### 80. How do you reconcile customer-specific customization with a shared product?

Tags: #CustomerExtensions #CompanyUnverified #AmazonRelevant

I separate reusable capabilities from local workflow rules and prefer configuration or stable extension interfaces where appropriate. I evaluate each customization's value, testing burden, upgrade impact, and long-term owner. I avoid spreading customer-specific branches throughout shared business logic.

For example, a configurable approval policy may support several customers, while a unique legacy adapter can remain isolated. I document compatibility expectations and validate upgrades against important extensions. Generalization should follow evidence of shared needs; speculative abstraction can be as costly as maintaining many uncontrolled forks.

### AI systems and FDE deployment scenarios (81–90)

#### 81. How would you design an enterprise RAG application?

Tags: #RAGArchitecture #CompanyUnverified #AmazonRelevant

I separate ingestion from serving. Ingestion obtains approved documents, parses and chunks them, captures provenance and permissions, and updates searchable representations. Serving authenticates the user, retrieves authorized evidence, constructs a bounded context, and generates an answer with source references or an explicit inability to answer.

I evaluate retrieval quality separately from answer quality and measure latency, cost, and successful user tasks. Updates, deletions, and access changes must propagate without allowing stale permissions to expose content. I also include source freshness, prompt-injection defenses, and a fallback or human escalation path for unsupported or consequential questions.

#### 82. How do you choose between keyword, vector, and hybrid retrieval?

Tags: #RetrievalDesign #CompanyUnverified #AmazonRelevant

Keyword retrieval can work well for exact identifiers, terminology, and lexical matches. Vector retrieval can capture semantic similarity but may miss exact constraints or return plausible yet irrelevant content. Hybrid retrieval combines signals, often followed by filtering or reranking when the benefit justifies the cost.

I compare approaches on representative queries and relevant evidence judgments rather than assuming semantic search always wins. I include permission-aware retrieval, index freshness, latency, and difficult cases such as codes or negation. The retrieval method should follow the user's information need and measured quality, not a preference for a particular database type.

#### 83. How would you design an AI agent that can update customer systems?

Tags: #AgentArchitecture #CompanyUnverified #AmazonRelevant

I model the agent as a workflow with bounded tools, explicit state, and durable progress where needed. The model may propose an action, but trusted application code validates its arguments, permissions, limits, and required approvals. I separate read-only operations from consequential writes and record action outcomes.

I design idempotency and reconciliation for retries and uncertain external responses. A customer document or tool result must not grant new authority. I also cap execution time and cost and provide a clear stop or handoff path. See [OWASP's prompt-injection guidance](https://cheatsheetseries.owasp.org/cheatsheets/LLM_Prompt_Injection_Prevention_Cheat_Sheet.html) for risks from untrusted model inputs.

#### 84. How do you control inference latency and cost?

Tags: #InferencePerformance #CompanyUnverified #AmazonRelevant

I measure time to first useful output, total completion time, token volume, model choice, retrieval, and tool calls. I reduce unnecessary context, bound output, and choose the least expensive configuration that meets evaluated quality. Parallel calls or caching help only when their semantics and workload justify them.

Streaming can improve perceived responsiveness without reducing total computation. I protect tenant budgets and concurrency and avoid sharing cached outputs across incompatible permission contexts. Every optimization needs a quality check because a cheaper or faster response that requires more correction may increase the cost per successful task.

#### 85. What does a safe model fallback strategy require?

Tags: #ModelFallback #CompanyUnverified #AmazonRelevant

I define which failures trigger fallback and whether the alternative meets the task's quality, privacy, residency, latency, and tool-use requirements. A fallback model may interpret instructions or structured outputs differently, so I evaluate the whole workflow rather than treating providers as interchangeable endpoints.

I bound attempts and avoid sending repeated requests into a failing dependency. If no approved alternative meets requirements, an explicit temporary failure or human handoff may be the correct behavior. I expose degraded operation where useful and track its frequency so availability improvements do not hide unacceptable quality loss.

#### 86. How do you release and monitor an AI system whose outputs vary?

Tags: #AIOperations #CompanyUnverified #AmazonRelevant

I version the model configuration, prompts, tools, retrieval settings, and evaluation data needed to interpret a release. I use representative offline checks and a bounded rollout, then monitor task quality, latency, cost, user corrections, and important failure categories. I protect sensitive content in diagnostic records.

I define rollback and stop criteria for the complete configuration, not just application code. A new model can interact differently with the same retrieval results or tools. Human feedback helps, but I also review whether users detect errors; low complaint volume alone does not prove correctness.

#### 87. How do you design an application for intermittent connectivity?

Tags: #OfflineFirst #CompanyUnverified #AmazonRelevant

I identify which reads and actions must work offline, which data can be stored locally, and which operations require current authority. I use durable local state and queued operations with stable identifiers, then synchronize through an authenticated protocol. I define conflict resolution, retry, and user-visible synchronization status.

For example, offline draft capture may be safe while a scarce-resource reservation needs an online authoritative decision. I address device loss, encryption, revocation limits, and stale data. “Offline capable” should name the supported workflow boundary rather than imply that every online guarantee remains available during disconnection.

#### 88. How would you design a bidirectional CRM integration?

Tags: #CRMIntegration #CompanyUnverified #AmazonRelevant

I assign authoritative ownership by entity or field, map stable identifiers, and define permitted changes in each direction. I use supported events or polling with checkpoints and reconciliation. I validate schemas and permissions and respect the external system's quotas and change processes.

I prevent update loops with provenance, versions, or comparison of meaningful changes. Concurrent edits need an explicit conflict policy rather than automatic overwrites. I also handle deletes and partial failures and show operators what is out of sync. Two working one-way connectors do not automatically form a correct bidirectional system.

#### 89. How do you migrate a customer system with minimal downtime?

Tags: #SystemMigration #CompanyUnverified #AmazonRelevant

I map data and behavior, establish compatibility requirements, and rehearse migration on representative data. I coordinate an initial copy with ongoing changes, reconcile results, and define a cutover point with clear write authority. I include dependencies, user communication, and support coverage.

Dual writes introduce partial failures and should not be assumed atomic. I prefer a controlled replication or handoff strategy with verification appropriate to the systems involved. The recovery plan must account for new writes after cutover; simply switching traffic back can lose or conflict with data created in the new system.

#### 90. How do you turn an FDE pilot into a production architecture?

Tags: #PilotToProduction #CompanyUnverified #AmazonRelevant

I compare the pilot's tested conditions with production requirements for users, scale, permissions, availability, support, and cost. I identify gaps in deployment, data handling, monitoring, recovery, testing, and ownership. I prioritize the gaps according to business impact and the planned rollout size.

I preserve the validated workflow while replacing temporary assumptions such as manual credentials or mocked integrations. A staged rollout provides evidence that production conditions are supported. I agree on acceptance criteria and operational handoff so “production ready” means more than a successful demonstration with a few cooperative users.

### End-to-end design exercises (91–100)

These designs are starting proposals under stated assumptions. In an interview, first confirm which features and scale matter, then trace the important success and failure paths. The companies in reported questions are the attributed interview companies, not the owners of every technology mentioned in an answer.

#### 91. Design a URL shortener.

Tags: #URLShortener #TeamApt #CandidateReported

**Attribution:** A [Glassdoor candidate-report page](https://www.glassdoor.com/Interview/Design-a-url-shortener-The-interwiew-was-for-45-60-minutes-unless-one-has-previously-practiced-the-question-which-defeat-QTN_6528702.htm) attributes this prompt to a TeamApt Senior Software Engineer interview. The report does not independently verify the interview or establish an FDE role or individual interviewer identity.

I clarify custom aliases, expiration, abuse controls, analytics, and read/write volume. A create API validates the destination and stores an alias-to-URL mapping in an authoritative database with a uniqueness constraint. Short codes can come from allocated identifiers or sufficiently large random values with collision retries. A redirect endpoint looks up the mapping, checks its state, and returns an appropriate redirect response.

I cache popular mappings when stale-data behavior is acceptable and record analytics asynchronously so tracking does not block redirects. I discuss enumeration, malicious destinations, deletion propagation, and hot links. Redirect cache policy must reflect whether mappings can change; permanent caching can make later revocation difficult. I measure redirect latency and cache-miss load and scale storage only when the workload requires it.

#### 92. Design a rate limiter allowing five requests per rolling 60-second window.

Tags: #RateLimiter #BlackbirdLabs #CandidateReported

**Attribution:** Adapted from a [Glassdoor candidate-report page](https://www.glassdoor.com/Interview/Design-a-rate-limiter-that-allows-5-requests-in-a-rolling-60-second-window-There-were-two-additional-requirements-that-wo-QTN_8991614.htm) attributed to a Blackbird Labs Senior Software Engineer interview. The source mentions additional unspecified requirements; they are not invented here. The attribution is not independently verified.

I first clarify whether the limit is per user, API key, or another identity and whether the rolling limit must be exact. For an exact small limit, I can keep accepted-request timestamps per key. In one atomic operation, remove timestamps at or before the window's lower boundary, count the remaining entries, and append the current accepted request only when fewer than five remain. I define boundary inclusion and use unique entries for simultaneous timestamps.

Multiple application instances need coordinated state for the same key or an explicit approximation. I expire inactive keys and use a consistent time source for enforcement. A token bucket permits different burst behavior and is not automatically equivalent to this strict rolling rule. I define behavior when the store is unavailable and explain the latency, availability, and enforcement tradeoffs of multi-region coordination.

#### 93. Design a real-time chat system.

Tags: #ChatSystem #CompanyUnverified #AmazonRelevant

I clarify one-to-one versus group chat, retention, attachments, offline delivery, and ordering expectations. Clients authenticate to connection gateways, while a message service checks membership and durably appends messages to a conversation history. I assign stable message identifiers and per-conversation ordering where required. Fan-out delivers updates to connected recipients, and offline users retrieve history later.

The sender receives a clearly defined acknowledgement, such as durable acceptance, rather than an ambiguous “delivered” label. Reconnecting clients use cursors to fetch missed messages, and retrying clients reuse identifiers to avoid duplicate effects. I distinguish ephemeral presence and typing indicators from durable history. Membership changes, blocked users, retention, and device synchronization require explicit policies, and a live connection never replaces the authoritative authorization check.

#### 94. Design a personalized news feed.

Tags: #NewsFeed #CompanyUnverified #AmazonRelevant

I clarify posting, follow relationships, ranking, privacy, deletion, and freshness. I store posts and relationships authoritatively and build a feed read model. Fan-out on write can make reads fast for ordinary publishers, while fan-out on read can avoid excessive write amplification for accounts with huge audiences. A hybrid approach can combine them based on measured workload.

I use stable pagination and define how ranking changes affect subsequent pages. Feed entries reference source content and its current visibility so deletions or permission changes do not rely solely on stale cached lists. Ranking can be a separate bounded stage with a simpler fallback. I measure publish-to-feed delay, read latency, fan-out backlog, and user-visible relevance while isolating very large publishers from the normal processing path.

#### 95. Design a multi-channel notification service.

Tags: #Notifications #CompanyUnverified #AmazonRelevant

I define email, SMS, or push requirements, urgency, preferences, quiet hours, and delivery expectations. A notification API validates an authorized request and durably records an intent with an idempotency key. Routing applies policy and schedules channel-specific jobs. Workers call providers under their quotas and record attempts, provider identifiers, and outcomes.

I distinguish accepted, provider-accepted, delivered, and user-read states because available evidence differs by channel. I recheck relevant preferences at sending time and design expiration so delayed notifications do not arrive after they stop being useful. Retries require provider-compatible idempotency or an explicit duplicate-risk policy. Webhooks need authenticity checks and deduplication. Provider failover must respect user consent and channel policy, and operational dashboards should expose stuck work and delivery failures.

#### 96. Design a distributed job scheduler.

Tags: #JobScheduler #CompanyUnverified #AmazonRelevant

I clarify one-time versus recurring jobs, timing precision, time zones, cancellation, workload, and delivery guarantees. A durable schedule store tracks the next eligible run. Scheduler instances claim due work through a coordinated lease or atomic transition and publish execution jobs. Each scheduled occurrence has a stable identifier, and workers record progress and completion durably.

I assume execution can be retried and make effects idempotent rather than claiming a lease guarantees exactly one execution. Fencing or equivalent state checks prevent stale owners from accepting conflicting updates. I define missed-run policy, daylight-saving behavior, overlap limits, and tenant fairness. Queue age, overdue schedules, and expired claims reveal failures. Recovery after downtime needs a bounded catch-up process so overdue work does not overwhelm the system.

#### 97. Design a ticket-booking system for limited seats.

Tags: #BookingSystem #CompanyUnverified #AmazonRelevant

I distinguish browsing approximate availability, holding seats, paying, and confirming a booking. The authoritative reservation service performs an atomic state transition so two customers cannot hold the same seat concurrently. Holds have an explicit expiration policy and identifiers. Cached seating views can be stale, but the reservation decision checks authoritative state.

Payment is a separate workflow with idempotent requests, callbacks, and reconciliation for uncertain outcomes. I coordinate confirmation with hold state so an expired reservation is not accidentally confirmed after another customer acquires the seat. Expiration workers use conditional transitions and cannot release a confirmed booking. During demand spikes, admission control or a waiting room protects the scarce-resource path. I test simultaneous attempts, duplicate callbacks, and crashes at each transition.

#### 98. Design a large-file upload and processing service.

Tags: #FileProcessing #CompanyUnverified #AmazonRelevant

I clarify file limits, supported types, resumability, processing stages, and privacy requirements. An authenticated API creates an upload session with scoped access to an object destination. The client uploads in bounded parts where supported, and the service verifies completion, size, and integrity before marking the object ready. Metadata tracks ownership and processing state.

A durable handoff schedules validation, scanning, extraction, and any transformation. Workers use idempotent stage identifiers and record failures separately from upload status. I restrict untrusted file processing and enforce resource limits. Abandoned uploads and intermediate objects need cleanup, while downstream effects must wait for the required validation. Status and retry APIs explain whether the client should resume uploading, wait for processing, or correct an unsupported file.

#### 99. Design an enterprise search service across several customer systems.

Tags: #EnterpriseSearch #CompanyUnverified #AmazonRelevant

I identify authoritative sources, query types, freshness, access rules, and expected volumes. Connectors ingest approved content and permission metadata through supported interfaces, normalize identifiers, and update a derived index. I track checkpoints, versions, deletes, and connector health. The query service authenticates users and applies tenant- and resource-appropriate access controls.

I use relevant lexical or semantic retrieval based on evaluation and return provenance and freshness information. Permission revocation must not depend only on eventually updated index metadata; sensitive result paths need authoritative or otherwise sufficiently current enforcement. I design reindexing and reconciliation without creating duplicate results or resurrecting deleted content. Connector quotas and failures are isolated so one source does not block every search, and partial results are communicated clearly.

#### 100. Design a near-real-time operational analytics dashboard.

Tags: #AnalyticsDashboard #CompanyUnverified #AmazonRelevant

I clarify which decisions the dashboard supports, how fresh the data must be, and what each metric means. Approved source events or CDC flow through durable ingestion into validated transformations and a serving store optimized for dashboard queries. I separate analytical load from transactional systems and keep enough lineage to explain a displayed value.

I define handling for duplicates, late events, corrections, and time zones. Aggregations need consistent windows and business definitions, and the interface should show freshness or degraded source status. I enforce tenant isolation through ingestion and serving, not only in the interface. Reconciliation compares displayed metrics with authoritative totals over suitable windows. I monitor ingestion lag, query latency, data quality, and cost, and use batch processing if it meets the actual decision deadline more simply.

## Practice and design review checklist

Practice ten questions at a time. Start with requirements and assumptions, give a clear proposal, then discuss an alternative and a concrete failure path. For the exercises, draw the design and trace one successful request, one concurrent request, and one retry after a crash.

Before ending an answer, check:

1. Is the target workflow and success criterion clear?
2. Are the load estimates and latency assumptions explicit?
3. Does every important record have an authoritative owner?
4. Are API contracts, permissions, and tenant boundaries defined?
5. What happens during a timeout, duplicate, concurrent update, or dependency outage?
6. Can the system recover data and reconcile uncertain external effects?
7. Which metrics would expose a bottleneck or incorrect outcome?
8. Who deploys, operates, supports, and pays for the solution?
9. What is intentionally excluded, and what evidence would justify the next architectural change?

Continue with [customer discovery](../02-customer-discovery/README.md), [backend APIs](../08-backend-apis/README.md), [databases](../09-databases/README.md), [customer deployment](../17-customer-deployment/README.md), and [production troubleshooting](../18-production-troubleshooting/README.md).
