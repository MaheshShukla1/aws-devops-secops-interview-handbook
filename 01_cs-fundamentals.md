# 🚀 Interview Handbook: CS Fundamentals, AWS, DevOps, Linux, Security & Automation

> Obsidian-ready. Q&A first, then practical reasoning and examples. ASCII diagrams where useful.


---

# 1) CS Fundamentals

## Section: OOPs

**Q1. What are the four pillars of OOP?**

**Answer (Natural):**  
Encapsulation (bundle data+methods), Abstraction (hide details), Inheritance (reuse behavior), Polymorphism (same interface, different behavior). These make code modular, testable, and extensible in real systems.


👉 **Example:** Python: override methods for runtime polymorphism.


---

**Q2. Class vs Object vs Instance?**

**Answer (Natural):**  
A class is a blueprint; an object/instance is a concrete realization with its own state. Classes define behavior once; instances let you scale behavior to many entities.


👉 **Example:** class Dog: pass -> d=Dog() creates an instance with independent attributes.


---

**Q3. Composition vs Inheritance—when to prefer which?**

**Answer (Natural):**  
Inheritance models **is-a**; composition models **has-a**. Prefer composition to reduce tight coupling and avoid deep hierarchies, making systems easier to change.


👉 **Example:** A Car has-an Engine (composition). A SportsCar is-a Car (inheritance).


---

**Q4. Overloading vs Overriding?**

**Answer (Natural):**  
Overloading: same method name, different signatures (compile-time; not native in Python). Overriding: subclass changes a parent method (runtime). Overriding supports polymorphism.


👉 **Example:** In Java: `print(int)` and `print(String)` overload. In Python, use default args or `functools.singledispatch`.


---

**Q5. What is SOLID and why does it matter?**

**Answer (Natural):**  
SOLID is a set of design guidelines (SRP, OCP, LSP, ISP, DIP) that reduce change impact and improve testability and extensibility.


👉 **Example:** Open-Closed: add new payment types by adding new classes, not editing existing ones.


---

**Q6. Interface vs Abstract Class?**

**Answer (Natural):**  
Interfaces define contracts (no state). Abstract classes provide partial implementation and state. Use an interface for capability sets, abstract class for a base with shared logic.


👉 **Example:** Java: `List` is an interface, `AbstractList` is an abstract class.


---

**Q7. Mutable vs Immutable objects?**

**Answer (Natural):**  
Immutable objects cannot change state after creation—easier to reason about and thread-safe by design; mutable ones can change and are often faster for in-place ops.


👉 **Example:** In Python, `tuple` is immutable; `list` is mutable.


---

**Q8. What is a design pattern?**

**Answer (Natural):**  
A reusable solution template to a recurring problem in a context. They accelerate design decisions and improve shared vocabulary.


👉 **Example:** Singleton (controlled instance), Strategy (pluggable behavior), Factory (centralized creation).


---

**Q9. What is duck typing?**

**Answer (Natural):**  
Behavioral typing: "If it quacks like a duck…" Interfaces are implied by methods, not class lineage—flexible for integration but requires good tests.


👉 **Example:** Any object with `.write()` can be passed to a logger.


---

**Q10. What are data classes/POJOs and why use them?**

**Answer (Natural):**  
Lightweight containers for data with minimal behavior—simplify serialization, equality, and construction.


👉 **Example:** Python: `@dataclass` for auto `__init__`, `__repr__`, `__eq__`.


---

## Section: DBMS

**Q11. What is normalization (1NF/2NF/3NF/BCNF)?**

**Answer (Natural):**  
Normalization reduces redundancy and anomalies. 1NF: atomic values; 2NF: remove partial dependency on composite keys; 3NF: remove transitive dependencies; BCNF: every determinant is a candidate key. Use until query patterns demand denormalization.


👉 **Example:** Customer & Orders split into tables linked by customer_id to avoid update anomalies.


---

**Q12. Primary Key vs Unique vs Foreign Key?**

**Answer (Natural):**  
Primary key uniquely identifies a row (one per table, not null). Unique enforces uniqueness (nullable). Foreign key references a parent key to ensure referential integrity.


👉 **Example:** orders.customer_id → customers.id (FK).


---

**Q13. ACID properties?**

**Answer (Natural):**  
Atomicity (all-or-nothing), Consistency (valid state transitions), Isolation (concurrent correctness), Durability (persist after commit). Critical for money transfers and inventory.


👉 **Example:** Bank transfer uses a transaction to update both accounts atomically.


---

**Q14. Indexes—how they work and trade-offs?**

**Answer (Natural):**  
Indexes (often B+Trees) speed lookups by key but cost writes and storage. Choose columns with high selectivity. Composite index order matters.


👉 **Example:** Index on (status, created_at) speeds `WHERE status='open' ORDER BY created_at`.


---

**Q15. Joins: INNER vs LEFT vs RIGHT vs FULL vs CROSS?**

**Answer (Natural):**  
INNER returns matches; LEFT all left rows + matches; RIGHT all right rows; FULL all rows with nulls for no match; CROSS Cartesian product.


👉 **Example:** LEFT join to include customers with zero orders.


---

**Q16. Transactions vs Autocommit?**

**Answer (Natural):**  
Autocommit writes immediately; explicit transactions group multiple statements for atomicity. Use `BEGIN…COMMIT` around multi-step changes.


👉 **Example:** Wrap inventory decrement + order creation in one transaction.


---

**Q17. Isolation levels (Read Uncommitted → Serializable)?**

**Answer (Natural):**  
Trade performance for correctness. Read Committed avoids dirty reads; Repeatable Read avoids non-repeatable reads; Serializable avoids phantoms but can reduce concurrency.


👉 **Example:** Analytics: Read Committed; financial ops: Serializable.


---

**Q18. OLTP vs OLAP?**

**Answer (Natural):**  
OLTP = many short transactions (operational DB). OLAP = analytical queries over large datasets (data warehouse). Different schemas and engines.


👉 **Example:** MySQL/Postgres for OLTP, Redshift/Snowflake/BigQuery for OLAP.


---

**Q19. Denormalization and materialized views—when?**

**Answer (Natural):**  
When read-heavy workloads need pre-joined or aggregated data for speed. Accept some write complexity or staleness.


👉 **Example:** Daily sales summary table refreshed hourly.


---

**Q20. Query optimization basics?**

**Answer (Natural):**  
Use `EXPLAIN`, proper indexes, avoid SELECT *, limit result sets, prefer set-based ops over row-by-row. Cache hot read paths.


👉 **Example:** Add covering index and rewrite correlated subquery into a join.


---

**Q21. NoSQL types and when to pick them?**

**Answer (Natural):**  
Key-Value (Redis), Document (MongoDB), Wide-Column (Cassandra), Graph (Neo4j). Choose based on access patterns, scale, and consistency requirements.


👉 **Example:** Session storage → Redis; product catalog → Document DB.


---

**Q22. Sharding vs Replication?**

**Answer (Natural):**  
Sharding splits data horizontally for scale; replication copies for HA and reads. Often used together.


👉 **Example:** User-id hash modulo N to shard; replicas for read scaling.


---

**Q23. Deadlocks—what and how to avoid?**

**Answer (Natural):**  
Cyclic waiting between transactions. Avoid by ordering locks consistently, keeping transactions short, using appropriate isolation, and handling retries.


👉 **Example:** Always lock accounts in ascending id order.


---

**Q24. Explain CAP theorem in practical terms.**

**Answer (Natural):**  
You can only fully have two of Consistency, Availability, Partition tolerance at once in a distributed system. In practice you design for partitions, then pick CP or AP per workload.


👉 **Example:** Banking (CP), social feed (AP).


---

**Q25. What is a covering index?**

**Answer (Natural):**  
An index that satisfies a query entirely (all selected/filtered columns are in the index), avoiding table lookups—fast but larger index size.


👉 **Example:** Index on (status, created_at, id) covers `SELECT id WHERE status='open' ORDER BY created_at`.


---

## Section: Operating Systems (OS)

**Q26. Process vs Thread vs Coroutine?**

**Answer (Natural):**  
Process has isolated memory; thread shares memory within a process; coroutine is cooperative user-space concurrency. Choose based on isolation, CPU vs I/O, and scheduling needs.


👉 **Example:** Python: threads for I/O, multiprocessing for CPU, asyncio for high concurrency I/O.


---

**Q27. Context switching—why is it expensive?**

**Answer (Natural):**  
Switching CPU from one thread/process to another saves/restores registers, TLB, cache effects. Excessive switching (thrashing) hurts throughput.


👉 **Example:** Batch small tasks into fewer context switches using worker pools.


---

**Q28. What is virtual memory & paging?**

**Answer (Natural):**  
Virtual memory abstracts physical RAM; paging moves fixed-size pages between RAM and disk. Enables isolation and larger address spaces, but paging to disk is slow.


👉 **Example:** High page faults → consider more RAM or memory-efficient data structures.


---

**Q29. Scheduling algorithms (RR, Priority, CFS)?**

**Answer (Natural):**  
Schedulers allocate CPU time fairly and responsively. RR gives time slices; Priority favors important tasks; Linux CFS balances fairness with weights.


👉 **Example:** Latency-sensitive services use higher priority; batch jobs lower.


---

**Q30. System calls vs library calls?**

**Answer (Natural):**  
Syscalls enter kernel mode (e.g., read/write/socket). Library calls run in user space and may wrap syscalls. Crossing boundary adds overhead.


👉 **Example:** `read(fd)` vs `memcpy()`.


---

**Q31. Mutex vs Semaphore vs RWLock?**

**Answer (Natural):**  
Mutex is mutual exclusion for one holder; semaphore is a counter for N permits; RWLock allows many readers or one writer. Pick RWLock for read-heavy workloads.


👉 **Example:** Protect shared cache map with RWLock.


---

**Q32. What is a zombie process?**

**Answer (Natural):**  
A process that has terminated but not reaped by its parent; remains as an entry with exit status. Avoid by wait()-ing on children.


👉 **Example:** Use `waitpid` or `Init` reparenting.


---

**Q33. Demand paging & copy-on-write?**

**Answer (Natural):**  
Pages are loaded lazily on access; COW duplicates pages only on write. Saves memory and speeds forks.


👉 **Example:** Forking a server process uses COW for efficiency.


---

**Q34. Filesystem journaling?**

**Answer (Natural):**  
Records intent (metadata/data) so recovery can replay or roll back after crashes. Improves consistency at slight write overhead.


👉 **Example:** ext4, XFS, NTFS use journals.


---

**Q35. What is NUMA and why care?**

**Answer (Natural):**  
Non-Uniform Memory Access: memory latency differs across CPU sockets. Pin threads and allocate memory close to CPU to reduce remote access latency.


👉 **Example:** Set CPU/memory affinity for DB servers.


---

**Q36. Signals—how used?**

**Answer (Natural):**  
Async notifications to processes (SIGTERM, SIGINT). Use for graceful shutdown and reload (SIGHUP).


👉 **Example:** Systemd sends SIGTERM, app flushes state then exits.


---

**Q37. Kernel space vs user space?**

**Answer (Natural):**  
User space is isolated; kernel space has full HW access. Drivers and syscalls are kernel; apps run in user.


👉 **Example:** Bugs in kernel modules can panic the OS.


---

**Q38. What is cgroups & namespaces (Linux)?**

**Answer (Natural):**  
Namespaces isolate views (PID, NET, MNT), cgroups control resource usage. Foundation of containers.


👉 **Example:** Docker uses namespaces + cgroups to sandbox processes.


---

**Q39. Paging vs swapping?**

**Answer (Natural):**  
Paging moves memory pages; swapping historically referred to moving entire processes. Modern systems page; heavy paging degrades performance.


👉 **Example:** High swap-in/out → memory pressure.


---

**Q40. Thrashing—symptoms & fix?**

**Answer (Natural):**  
Excessive paging due to insufficient RAM or working set > memory. Fix with more RAM, tune caches, reduce concurrency, or optimize algorithms.


👉 **Example:** DB server constantly swapping during peak → add RAM and tune buffer/cache.


---

## Section: Computer Networks

**Q41. Walkthrough: what happens when you hit https://example.com?**

**Answer (Natural):**  
DNS resolution, TCP 3-way handshake, TLS handshake, HTTP request/response, server renders, browser parses & renders. Each step can fail—use logs and tracing to debug.


👉 **Example:** CDN caches assets; TLS uses certificates to authenticate server.


---

**Q42. TCP vs UDP—when do you use each?**

**Answer (Natural):**  
TCP is reliable, ordered, connection-oriented; UDP is connectionless, faster, with no guarantees. Use UDP for real-time (VoIP/streaming) with app-level recovery.


👉 **Example:** gRPC uses HTTP/2 over TCP; DNS uses UDP with fallbacks.


---

**Q43. 3-way handshake & TLS handshake basics?**

**Answer (Natural):**  
TCP: SYN → SYN-ACK → ACK. TLS: negotiate protocol/cipher, exchange keys, verify cert, then encrypt. Latency matters; use TLS session resumption and HTTP/2/3.


👉 **Example:** Enable TLS 1.2+ and OCSP stapling.


---

**Q44. Subnetting: CIDR /24 vs /16?**

**Answer (Natural):**  
/24 = 256 addresses; /16 = 65,536. Smaller subnets reduce broadcast domain and blast radius. Plan VPC CIDRs to avoid on-prem overlaps.


👉 **Example:** 10.0.0.0/16 split into many /24 subnets.


---

**Q45. NAT vs PAT?**

**Answer (Natural):**  
NAT maps one address space to another; PAT (NAT overload) maps many internal IP:port pairs to a single public IP. Common for egress in clouds.


👉 **Example:** AWS NAT Gateway implements outbound-only internet access for private subnets.


---

**Q46. DNS record types (A/AAAA/CNAME/ALIAS/TXT/MX/SRV)?**

**Answer (Natural):**  
A/AAAA: IPv4/IPv6, CNAME: name alias, ALIAS: zone apex alias (Route 53), TXT: metadata (SPF/verification), MX: mail routing, SRV: service discovery.


👉 **Example:** Route 53 supports ALIAS to AWS resources.


---

**Q47. HTTP 1.1 vs 2 vs 3?**

**Answer (Natural):**  
HTTP/2 multiplexes over a single TCP connection; HTTP/3 uses QUIC over UDP reducing head-of-line blocking. Impacts latency and mobile performance.


👉 **Example:** CDNs terminate HTTP/3 at edge.


---

**Q48. Load balancer vs reverse proxy?**

**Answer (Natural):**  
Reverse proxy sits in front of servers and forward requests; load balancer distributes across many backends. Many products do both.


👉 **Example:** Nginx/Envoy as reverse proxy and L7 load balancer.


---

**Q49. CIDR aggregation and route tables?**

**Answer (Natural):**  
Aggregating routes reduces table size and improves performance. In clouds, route tables determine next hop (IGW, NAT, peering).


👉 **Example:** 10.0.0.0/16 to TGW, specific /24 to NAT.


---

**Q50. MTU & MSS—why do packet drops happen?**

**Answer (Natural):**  
If MTU mismatch (e.g., VPN encapsulation) causes fragmentation or drops. PMTUD helps, but firewalls may block ICMP. Tune MSS clamping on tunnels.


👉 **Example:** Set MSS to 1360 on IPSec tunnels to avoid fragmentation.


---

**Q51. Sticky sessions—good or bad?**

**Answer (Natural):**  
They simplify session affinity but reduce load distribution and resilience. Prefer stateless sessions with external stores.


👉 **Example:** Use JWTs or Redis session store.


---

**Q52. Health checks (L4 vs L7)?**

**Answer (Natural):**  
L4 checks port; L7 checks HTTP path/headers and can assert app correctness. Use L7 to detect partial outages.


👉 **Example:** Check `/healthz` returning 200 plus DB connectivity.


---

**Q53. CDN vs Edge compute?**

**Answer (Natural):**  
CDN caches static; edge compute runs code near users (e.g., CloudFront Functions/Lambda@Edge) to cut latency.


👉 **Example:** Rewrite URLs and auth at the edge before origin.


---

**Q54. Zero trust networking overview?**

**Answer (Natural):**  
Never trust, always verify—strong identity, least privilege, continuous authorization, micro-segmentation, and telemetry.


👉 **Example:** mTLS between services + policy engine for authorization.


---

**Q55. Observability in networks?**

**Answer (Natural):**  
Metrics (latency, errors), logs (access, firewall), traces (distributed). SLOs tie to user-perceived latency and availability.


👉 **Example:** Use OpenTelemetry + service mesh to trace cross-service latency.


---
