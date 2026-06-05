UNIT I – DISTRIBUTED DATABASES: ARCHITECTURE & DESIGN
1. Distributed Data Processing (DDP)
Definition: Processing of data across multiple interconnected computers (sites), not just one central machine.

Relation to DDBS: DDP is the environment (hardware + network). DDBS is the software managing distributed data.

2. What is a DDBS?
Distributed Database (DDB): A single logical database that is physically spread over multiple sites connected by a network.
Distributed DBMS (DDBMS): Software that manages the DDB and makes distribution transparent to the user.

Analogy: Centralized = one restaurant kitchen. Distributed = a food chain (each branch has its own kitchen, but menus are unified).

3. Advantages of DDBS (memorise at least 5)
Reliability & Availability – One site fails, others continue.

Improved Performance – Data near users → faster access.

Scalability – Add sites without shutdown.

Modularity – Upgrade sites individually.

Economic – Many small computers cheaper than one supercomputer.

Autonomy – Local control over local data.

4. Disadvantages of DDBS
Complexity – Design, debugging, maintenance harder.

Security – More access points.

Integrity control – Global constraints difficult.

Distributed query processing – Joins across sites slow.

Network dependence – Network failures stop communication.

Deadlocks – Possible across multiple sites.

5. Problem Areas in DDBS (exam list)
Distributed database design (fragmentation, replication, allocation)

Distributed query processing & optimisation

Distributed transaction management (atomicity)

Distributed concurrency control

Distributed recovery

Security across sites

Heterogeneity (different hardware, OS, DBMS)

6. Distributed DBMS Architecture
6.1 Transparencies (Hide distribution details from users)
Transparency	What it hides	Example
Fragmentation	How a table is split	User queries global table, system combines fragments
Location	Where fragments are stored	No need to know fragment at Site A or B
Replication	Existence of multiple copies	Update one logical row, all replicas updated
Failure	Site/network failures	Query still works using another replica
Naming	Global unique naming of objects	System maps logical name to physical location
Mnemonic: F L R F N (Fragmentation, Location, Replication, Failure, Naming)

6.2 Reference Architecture (ANSI/SPARC distributed)
text
┌──────────────────────────────┐
│      External Level          │  ← User views (multiple)
└──────────────┬───────────────┘
               │
┌──────────────▼───────────────┐
│   Global Conceptual Schema   │  ← One logical whole database
└──────────────┬───────────────┘
               │
┌──────────────▼───────────────┐
│ Fragmentation & Allocation   │  ← How to break & where to put
└──────────────┬───────────────┘
               │
┌──────────────▼───────────────┐
│  Local Conceptual Schemas    │  ← Each site’s fragment description
└──────────────┬───────────────┘
               │
┌──────────────▼───────────────┐
│   Local Internal Schemas     │  ← Physical storage per site
└──────────────────────────────┘
Global Conceptual Schema: Logical description of entire DDB as if not fragmented.

Fragmentation Schema: Defines how each global relation is split (horizontal/vertical/mixed).

Allocation Schema: Defines which fragment resides at which site.

Local Schemas: Each site’s own view (in its own DBMS format).

6.3 Global Directory Issues
The global directory maps fragments to locations and replicas.
Problems:

Size (can become huge)

Replication consistency

Performance overhead (lookups)

Single point of failure if centralised

Update cost if replicated

Directory Strategy	Pros	Cons
Centralised	Simple, consistent	Bottleneck, single failure
Replicated (full copy each site)	Fast local lookup	Expensive updates
Partitioned (each site knows local)	Small size	Complex global queries
7. Distributed Database Design
7.1 Alternative Design Methodologies
Methodology	When to use	Process
Top‑Down	New system from scratch	Global schema → fragment → allocate
Bottom‑Up	Integrating existing DBs	Local schemas → map to global schema
7.2 Fragmentation
Definition: Splitting a global table into smaller pieces (fragments), each stored at one or more sites.

Why fragment?

Locality: put data where used

Security: separate sensitive columns

Parallel processing

Reduced network traffic

Types:

Type	Operation	Result	Example (Employee table)
Horizontal	Select (σ) rows	Subset of rows	dept = 'Sales'
Vertical	Project (π) columns	Subset of columns	emp_id, name, dept
Mixed	σ then π (or vice versa)	Small pieces	First by city, then by salary columns
Rules for fragmentation:

Completeness: Every attribute/row belongs to a fragment.

Disjointness: (except key in vertical) No overlap.

7.3 Replication
Definition: Storing the same fragment at multiple sites.

Type	Description	Pros	Cons
Full replication	Whole DB at every site	Very fast reads, fault‑tolerant	Slow writes, huge storage
Partial replication	Some fragments replicated	Balanced cost/performance	Design complexity
No replication	Each fragment once	Simple writes, minimal storage	Loss if site fails
Role in exam answers: Replication helps read‑intensive apps; hurts write‑intensive apps.

7.4 Data Allocation
Definition: Decision of which site stores which fragment (and replicas).

Strategy	Meaning
Centralised	All fragments on one site (no distribution)
Partitioned	Each fragment exactly one site (no replication)
Replicated	Every site has full copy
Selective/Hybrid	Some fragments partitioned, some replicated (most practical)
Factors influencing allocation:

Query frequency from each site

Update frequency

Storage capacity

Site reliability

Network latency

Exam tip: The three design dimensions = FRA (Fragmentation, Replication, Allocation).

UNIT I – KEY TERMS SUMMARY TABLE
Term	Definition
DDBS	Logically integrated, physically distributed database + management software
DDP	Processing data across multiple computers
Transparency	Hiding distribution from user
Fragmentation	Splitting table into rows/columns pieces
Replication	Storing copies of fragments at multiple sites
Allocation	Placing fragments onto sites
Global Directory	Maps fragments to locations
Top‑Down Design	Global → fragment → allocate
Bottom‑Up Design	Local → global integration
