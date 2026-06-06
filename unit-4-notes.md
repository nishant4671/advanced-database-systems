# Unit IV – Advanced Concepts in Transaction Management (6 Hours)

## Transaction Management

### ACID Properties
- **Atomicity** – Transaction either commits fully or aborts (no partial effects).
- **Consistency** – Transaction transforms database from one consistent state to another.
- **Isolation** – Concurrent transactions appear as if executed serially.
- **Durability** – Committed changes persist despite failures.

### Pessimistic Locking
- Locks acquired before accessing data.
- **Lock types** – Shared (read), Exclusive (write).
- **Two‑Phase Locking (2PL)** – Growing phase (acquire locks), shrinking phase (release locks). Ensures conflict serialisability.
- **Rigorous 2PL** – Release all locks only after commit/abort – avoids cascading aborts.
- **Deadlock** – Solved by timeout or detection (wait‑for graph).

### Optimistic Locking
- No locks during transaction execution.
- Three phases: **read** (record changes in workspace), **validation** (check conflicts with committed transactions), **write** (commit if validation passes).
- Best for low‑conflict workloads (e.g., most reads, few writes).
- Validation rules (timestamp ordering) – ensure serialisability.

### Flat Transactions
- Single sequence of operations, no nesting.
- Limitations: no partial rollback, no internal parallelism, locks held for entire duration.

### Nested Transactions
- Transaction can contain sub‑transactions (children).
- **Properties** – Commit of a child is provisional; if parent aborts, all children abort; if a child aborts, parent can retry or continue.
- **Locking** – Child inherits locks from parent, can lock more finely.
- **Advantages** – modularity, finer failure recovery, intra‑transaction parallelism.

### Deadlock Detection and Management

#### Centralised Algorithms (single DB)
- **Wait‑for graph (WFG)** – Nodes = transactions; edge T1→T2 if T1 waits for T2. Cycle = deadlock.
- Victim selection – choose transaction with smallest cost (age, number of locks, already restarted count).
- Recovery – abort victim, release locks, rollback.

#### Distributed Deadlock Detection
- **Hierarchical** – Each site has local WFG; global controller builds global graph.
- **Path‑pushing** – Send probe messages (e.g., Obermarck’s algorithm).
- **Edge‑chasing** – Distinguish deadlock from false cycles.
- **Timeout** – simpler but may cause unnecessary aborts.

### Recovery Methods
- **Logging** – Write‑ahead logging (WAL): log record written before data page.
- **Undo logging** – restore previous value on abort.
- **Redo logging** – replay committed operations after crash.
- **Undo/Redo logging** – for media failure.
- **Checkpoint** – synchronise log and database to bound recovery time.
- **ARIES** (Algorithm for Recovery and Isolation Exploiting Semantics) – uses LSN, dirty page table, fuzzy checkpoint, and three passes (analysis, redo, undo).

## Concurrency Control and Reliability in Distributed Databases

### Centralised vs Distributed Concurrency Control
| Aspect | Centralised | Distributed |
|--------|-------------|--------------|
| Lock manager | Single site | Multiple sites, distributed locks |
| Deadlock detection | Local WFG | Distributed WFG or timeout |
| Timestamp generation | Single counter | Global unique timestamps (e.g., Lamport clocks) |
| Data location transparency | No | Yes – fragmentation & replication |

### Distributed Concurrency Control Algorithms

1. **Distributed Locking**  
   - Locks managed by local lock managers at the site where data resides.  
   - Centralised lock manager (single coordinator) – bottleneck.  
   - Primary copy locking – one copy of each data item is designated as primary; all locks on that copy.

2. **Timestamp Ordering (TO)**  
   - Each transaction gets a unique timestamp (e.g., using a global sequence number).  
   - At each data item, remember `read_ts` and `write_ts`.  
   - If `TS(T) < write_ts(item)`, abort T (must be rolled back).  
   - If `TS(T) < read_ts(item)` for a write, abort T.  
   - **Distributed TO** – timestamps must be globally coordinated; may use Lamport timestamps or true time (Spanner).

3. **Optimistic Concurrency Control (Distributed)**  
   - Validation phase becomes global: check all accessed replicas for conflicts.  
   - Broadcast validation requests, collect votes.  
   - High network overhead.

### Deadlock Management in DDBSs
- **Centralised detection** – one node builds global WFG – single point of failure.  
- **Distributed detection** – each site detects local cycles; probes passed to detect global cycles.  
- **Obermarck’s algorithm** – each transaction sends a probe that carries its ID; if a probe returns to sender, deadlock exists.  
- **Timeout** – after waiting > threshold, transaction aborts itself; simple but may kill non‑deadlocked transactions.

### Reliability Issues in DDBSs

#### Types of Failures
- **Transaction failures** – logical errors, system errors (deadlock, division by zero).  
- **Site failures** – crash of a computer (memory loss, power outage).  
- **Media failures** – disk corruption, head crash.  
- **Communication failures** – lost messages, network partition.

#### Reliability Techniques
- **Replication** – maintain multiple copies of data.  
- **Fail‑stop model** – assumption: a failed site simply stops; does not act maliciously.  
- **Election algorithms** – choose a new coordinator after failure (Bully, Ring).  
- **Majority voting** – read/write quorums to ensure consistency.

#### Commit Protocols
- **Two‑Phase Commit (2PC)**  
  - Coordinator asks participants to prepare (vote yes/no).  
  - If all yes, coordinator sends commit; else abort.  
  - Problem: blocking if coordinator fails.  
- **Three‑Phase Commit (3PC)**  
  - Adds pre‑commit phase; avoids blocking under certain failure models.  
  - More messages, rarely used in practice.  
- **Paxos Commit** – uses consensus to replace coordinator; non‑blocking.

#### Recovery Protocol
- **Undo/Redo logging** extended to multiple sites.  
- **Replicated logs** – each site keeps its own log.  
- **Global checkpoint** – coordinate checkpoints across sites.  
- After a site failure:  
  1. Redo committed transactions that were in the log but not applied.  
  2. Undo transactions that were active at crash.  
- After network partition: use **availability vs consistency** trade‑offs (CAP theorem – see Unit VI).