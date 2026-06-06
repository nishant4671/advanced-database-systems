# Unit V – Advanced Querying and Information Retrieval (6 Hours)

## Decision Support Systems (DSS)
- Systems that help make business decisions using historical and analytical data.
- Contrast with OLTP (Online Transaction Processing) – short queries, high concurrency.
- DSS involves complex queries, aggregations, large scans.

## Data Analysis and OLAP (Online Analytical Processing)

### OLAP Concepts
- **Multidimensional data model** – facts (measures) and dimensions (attributes for grouping).
- **OLAP operations**:
  - **Roll‑up** – aggregate to a higher level (e.g., from month to year).
  - **Drill‑down** – go from higher to lower level.
  - **Slice** – fix one dimension (e.g., year = 2025).
  - **Dice** – restrict multiple dimensions.
  - **Pivot** – rotate the view (cross‑tabulation).

### Storage Modes
- **MOLAP** (Multidimensional OLAP) – pre‑computed cubes, fast queries, high storage.
- **ROLAP** (Relational OLAP) – star or snowflake schemas, slower but scalable.
- **HOLAP** – hybrid.

## Data Mining
- Discovery of hidden patterns and knowledge from large data.
- **Common tasks**:
  - **Classification** (e.g., decision trees, SVM) – assign class labels.
  - **Clustering** (e.g., k‑means, DBSCAN) – group similar objects.
  - **Association rule mining** (Apriori, FP‑Growth) – find itemsets that co‑occur.
  - **Regression** – predict numeric values.
  - **Outlier detection** – find rare events (fraud, intrusion).

## Data Warehousing
- Centralised repository integrating data from multiple operational sources.
- **ETL process** – Extract (from sources), Transform (clean, convert, deduplicate), Load (into warehouse).
- **Star schema** – central fact table connected to dimension tables (denormalised).
- **Snowflake schema** – normalised dimensions.
- **Data mart** – subset of warehouse for a specific department.

## Information Retrieval Systems (IRS)
- Systems that locate documents relevant to a user query (e.g., web search, digital libraries).
- **Key components**:
  - **Crawling** – gather documents from web.
  - **Indexing** – build inverted index (term → list of document IDs and positions).
  - **Ranking** – order results by relevance.
- **Retrieval models**:
  - **Boolean model** – exact match with AND/OR/NOT (no ranking).
  - **Vector Space Model** – documents and queries as vectors; cosine similarity.
  - **Probabilistic model** – BM25, language models.
  - **Neural IR** – dense embeddings (BERT, ColBERT).
- **Evaluation metrics** – Precision, Recall, F1, MAP, NDCG.

## Semantic Search
- Search based on meaning, not just keywords.
- Uses **knowledge graphs** (e.g., Wikidata, Google Knowledge Graph) to understand entities and relationships.
- Techniques: query expansion with synonyms, entity linking, natural language understanding (NLU), graph traversal.
- Example: “presidents of countries bordering India” – requires geographic relation.

## Database Tuning and Performance

### Benchmarking
- Standardised tests to measure and compare DBMS performance.
- Types: **operational** (OLTP), **analytical** (OLAP), **mixed** (HTAP).

### TPC Benchmarks
- **TPC‑C** – OLTP benchmark (order‑entry environment).
- **TPC‑H** – decision support (ad‑hoc queries, concurrent updates).
- **TPC‑DS** – big data analytics (supports SQL on structured/unstructured data).
- Metrics: **transactions per second (tpsQ)**, **price/performance** ($/tpsQ).

### Object‑Oriented Benchmarks
- **OO7** – for OODBMS (e.g., ObjectStore, Versant).
- **Bucky** – for object‑relational mapping.
- Focus on traversal of object graphs, method calls, inheritance.

### TP Monitors (Transaction Processing Monitors)
- Middleware that coordinates transactions across multiple resources (databases, queues).
- Examples: Tuxedo, CICS, Microsoft Transaction Server.
- Features: connection pooling, transaction routing, two‑phase commit.

### TPC and Wisconsin Benchmarks
- **Wisconsin Benchmark** – early relational benchmark (1980s) using a standard table and 20 queries.
- **TPC‑A/B** – legacy; replaced by TPC‑C.

### Performance Measurement
- **Metrics** – throughput (queries per second), latency (response time), resource utilisation (CPU, I/O, memory, network).
- **Tools** – `pgbench` (PostgreSQL), `sysbench`, `HammerDB`, `JMeter`.

### Performance Tuning
- **Logical tuning** – index selection (B‑tree, hash, bitmap, covering indexes), materialised views, denormalisation.
- **Physical tuning** – partitioning (range, list, hash), buffer pool size, parallelism degree.
- **Query tuning** – rewrite queries (avoid functions on indexed columns, use join order hints).
- **Automatic tuning** – Oracle Automatic SQL Tuning, Microsoft Database Engine Tuning Advisor.