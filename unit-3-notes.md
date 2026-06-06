# Unit III – Parallel Database Management System (6 Hours)

## Introduction

### Types of Parallelism in Database Systems
1. **I/O Parallelism** – Splits data across multiple disks (e.g., RAID, partitioning) to reduce I/O bottlenecks.
2. **Inter‑Query Parallelism** – Different queries run simultaneously on different processors. Each query is sequential internally.
3. **Intra‑Query Parallelism** – A single query is broken into subtasks that run in parallel.
   - **Intra‑operation parallelism** – Parallel execution of a single operator (e.g., parallel sort, parallel join).
   - **Inter‑operation parallelism** – Parallel execution of different operators in a query plan (pipeline or independent).

### Parallel Query Processing
- Goal: Reduce response time and increase throughput.
- Key techniques: data partitioning (range, hash, round‑robin), parallel algorithms for relational operators.

### Multiprocessor Architectures
- **Shared Memory** – All CPUs share common RAM. Easy to program, but limited scalability.
- **Shared Disk** – Each CPU has private memory, all share disk array. Good for clusters, but disk contention.
- **Shared Nothing** – Each node has its own CPU, memory, and disks. Communication via network. Best scalability (used in Teradata, Greenplum, Spark).

### Parallel Relational Operators
- **Parallel Scan** – Each partition scanned in parallel.
- **Parallel Sort** – Range or hash partitioning → local sort → merge.
- **Parallel Join**:
  - **Partitioned Hash Join** – Hash both relations on join attribute, then join matching partitions in parallel.
  - **Parallel Nested Loops** – Outer relation partitioned, inner relation replicated or partitioned.
  - **Parallel Sort‑Merge** – Sort both relations in parallel, then merge.

### Parallelism in Main‑Memory DBMS
- Data resides in RAM, so I/O is not a bottleneck.
- Parallelism focuses on CPU efficiency: vectorised execution, multi‑core processing, lock‑free data structures.
- Example: HyPer, Hana – use fine‑grained parallelisation of operators (e.g., parallel hash join on NUMA‑aware memory).

### Parallel Handling of Integrity Constraints
- **Primary/Unique keys** – Parallel checking: partition the data, check uniqueness inside each partition, then check cross‑partition duplicates using a distributed hash table.
- **Foreign keys** – Parallel join between referencing and referenced tables to verify existence.
- **Check constraints** – Partition the table and evaluate the predicate on each partition in parallel.

### Integrated I/O Parallelism
- Data is striped across multiple disks (e.g., RAID 0, 1+0).
- Query executor schedules I/O requests to all disks simultaneously.
- Example: Oracle Exadata – smart scans push predicates to storage cells, reducing data movement.

## Parallel Query Processing and Optimization

### Inter‑Query Parallelism
- Multiple independent queries run concurrently.
- Benefits: increases system throughput.
- Challenges: resource contention (CPU, I/O, memory), scheduling policies (fairness, priority).

### Intra‑Query Parallelism
- Single query split into parallel tasks.
- **Intra‑operation** – e.g., parallel hash join: build and probe phases partitioned.
- **Inter‑operation** – e.g., pipeline parallelism: while one operator produces rows, next operator consumes them.

### Objectives of Parallel Query Optimization
- Minimise response time for a single query.
- Maximise throughput for a mixed workload.
- Balance load across all nodes.
- Minimise data movement (communication cost).

### Parallel Query Optimisation
- Extends cost‑based optimisation with **parallelism cost model**:
  - Degree of parallelism (DOP).
  - Startup cost, communication cost, data skew.
- Search space includes parallel plans (e.g., exchange operators: gather, repartition, broadcast).
- **Dynamic optimisation** – adjust DOP at runtime based on actual data distribution.

### Load Balancing
- **Static partitioning** – range/hash based on statistics; may suffer from skew.
- **Dynamic load balancing** – work stealing (idle processor takes work from busy ones), adaptive repartitioning (e.g., skew‑handled hash join).
- Tools: round‑robin assignment of small tasks, virtual processors.

### Parallelism in Join Queries
- **Parallel sort‑merge join** – both relations partitioned on join key, sorted locally, then merged.
- **Parallel hash join** – hash partitioning (if join key is the partition key) or repartitioning (if not).
- **Broadcast join** – one relation small enough to broadcast to all nodes, then each node performs local join.
- **Fragment‑and‑replicate join** – for non‑equi joins or without a good partition key.

### Testing the Quality of Query Optimisation
- **Execution observability** – profile parallel operators (time per thread, data skew, network bytes).
- **Comparing real vs estimated** – row count, CPU time, I/O.
- **Scalability tests** – measure speedup (fixed data, more nodes) and scaleup (more data, proportional nodes).
- **Tools** – Explain plan with parallel details, performance dashboard (e.g., PostgreSQL `EXPLAIN ANALYZE`).