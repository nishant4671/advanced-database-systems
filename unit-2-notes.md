UNIT II – DISTRIBUTED QUERY PROCESSING & OPTIMIZATION
1. Problem of Query Processing in DDBS
Problem	Explanation
Data distribution	Fragments spread across sites
Replication	Multiple copies – which to use?
Network latency	Moving data is slow
Join across sites	Tables from different sites must meet
Unknown statistics	Remote fragment sizes unknown
Cost complexity	Need to minimise I/O + CPU + network
Core goal: Minimise total cost while returning correct results.

2. Distributed Query – Definition
A query that accesses data from multiple fragments located on different sites.

3. Distributed Query Processing Methodology (Flowchart)
text
User SQL query
       │
       ▼
┌──────────────┐
│ Decomposition│ → Relational algebra tree (canonical)
└──────────────┘
       │
       ▼
┌──────────────┐
│Data Localisa-│ → Replace global relations by fragment expressions
│    tion      │   (union for horizontal, join for vertical)
└──────────────┘
       │
       ▼
┌──────────────┐
│Global Optimis-│ → Choose best join order, site selection, semi‑join
│    ation     │   Minimise network cost
└──────────────┘
       │
       ▼
┌──────────────┐
│Local Optimisa-│ → Each site uses its own DBMS optimiser
│    tion      │   (indexes, access paths)
└──────────────┘
       │
       ▼
    Execution
4. Query Decomposition (First phase)
Steps (4):

Normalisation – Convert to normal form (e.g., predicate canonical).

Analysis – Check semantic correctness (attributes exist, types match).

Simplification – Remove redundant conditions (e.g., age>20 AND age>30 → age>30).

Restructuring – Transform to relational algebra tree.

Output: Canonical (initial) query tree.

5. Translation of Global Queries to Fragment Queries (Data Localisation)
Fragmentation Type	Fragment Query Construction
Horizontal	Union of fragments satisfying the selection condition
Vertical	Join of fragments on primary key (if query needs attributes from multiple fragments)
Mixed	Union of joins (or join of unions) depending on placement
Example – Horizontal:
Global: Employee fragmented into E_IT (dept='IT') and E_HR (dept='HR').
Query: SELECT * FROM Employee WHERE dept='IT' → Fragment query: SELECT * FROM E_IT

Example – Vertical:
Employee vertically fragmented: V1(emp_id, name), V2(emp_id, salary).
Query: SELECT name, salary FROM Employee WHERE emp_id=101 → Join V1 and V2 on emp_id.

6. Distributed Query Optimization
6.1 Objectives of Query Optimization (minimise)
Response time – from submit to result.

Resource consumption – CPU + I/O + memory.

Network traffic – messages and bytes.

Processing cost per site.

Trade‑off: Response time vs. resource consumption.

6.2 Factors Governing Query Optimization
Fragment statistics (size, cardinality, distributions)

Network speed & latency

Site processing power (CPU, memory)

Replication (choose cheapest copy)

Join methods (nested loop, hash, sort‑merge)

Available indexes

Cost model (formula with weights)

6.3 Ordering of Fragment Queries (Join Ordering)
Key principle: Reduce intermediate result size early.

Semi‑join (important for exam):

Send only the joining column of one relation to the other site.

Get back only matching rows (not full relation).

Then perform final join.

Benefits: Drastically reduces network transfer when selectivity is low.

Join ordering strategies:

Strategy	Description
Left‑deep tree	Join one table at a time, left to right. Simple.
Bushy tree	Join pairs in parallel. Good for distributed.
Greedy	Pick cheapest join next. Fast, not optimal.
Dynamic programming	Explore all orders (optimal for small number of fragments).
6.4 Optimization of Join Operation (Methods)
Method	Procedure	Best when
Ship‑whole	Send entire smaller relation to larger relation’s site	One relation is very small
Semi‑join	Send join column → get matches → then join	Low selectivity (few matches)
Hash‑partition	Hash both on join attr, distribute to same sites	Both relations large
Broadcast join	Send small relation to all sites holding parts of large	Many fragments of large relation
6.5 Load Balancing
Definition: Distributing processing workload across sites to avoid any single site becoming a bottleneck.

Techniques:

Fragment shipping to less busy sites

Query splitting into parallel sub‑queries

Adaptive routing (choose site based on current load)

Replica selection (read from least loaded replica)

Trade‑off: Load balancing may increase network traffic.

6.6 Distributed Query Optimization Algorithms
Algorithm	Approach	Characteristics
System R*	Dynamic programming	Optimal join order for ≤10 fragments, exponential cost
SDD-1	Semi‑join reduction	Minimises data transfer before joins, good for many joins
INGRES	Tuple substitution	Historical, simple
Hill‑climbing	Iterative improvement	May get stuck in local optimum
Genetic	Population evolution	For very large number of fragments
Exam tip: Remember System R* (optimal, exponential) and SDD-1 (semi‑join‑based).

6.7 Example Walkthrough (Join Optimisation)
Schema:
Customer(cust_id, name, city) – horizontal fragments by city (Delhi, Mumbai).
Order(order_id, cust_id, amount) – horizontal fragments by cust_id’s city (aligned).

Query: SELECT name, amount FROM Customer, Order WHERE Customer.cust_id = Order.cust_id AND city='Delhi'

Optimisation:

Data localisation: Use only Delhi fragment of Customer and corresponding Order fragment (same city).

If fragments co‑located → local join (no network cost).

If not aligned: choose to ship smaller fragment to other site.

Load balancing: if Delhi site overloaded, ship both to a third idle site.

UNIT II – KEY TERMS SUMMARY TABLE
Term	Definition
Distributed query	Query accessing multiple fragments on different sites
Query decomposition	SQL → relational algebra tree
Data localisation	Replace global relations with fragment expressions
Semi‑join	Send join column, then matching tuples only
Global optimisation	Choose join order, site selection, semi‑join usage
Local optimisation	Site‑level optimisation (indexes, access paths)
Load balancing	Spread workload across sites to avoid hotspots
System R*	Dynamic programming algorithm for optimal join order
SDD-1	Semi‑join reduction algorithm
SELF‑TEST QUESTIONS (Both Units)
Unit I
List three transparencies and explain one with an example.

Draw the reference architecture of a DDBS.

Compare horizontal and vertical fragmentation.

What are the advantages of replication? When is it harmful?

What factors influence data allocation?

Unit II
Draw the distributed query processing methodology flowchart.

What is the purpose of data localisation? Give an example.

How does a semi‑join reduce network cost?

Compare System R* and SDD-1.

What is load balancing? Name two techniques.

Answers (short)
See previous detailed answers in the lesson.

