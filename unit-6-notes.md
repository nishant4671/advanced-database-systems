# Unit VI – Big Data, NoSQL and Vector Databases (6 Hours)

## What is NoSQL?
- **Not Only SQL** – databases designed for high scalability, flexible schemas, and distributed architectures.
- Do not require fixed table schemas and avoid joins (use denormalisation).

## Why NoSQL?
- Limitations of RDBMS:
  - Rigid schemas make evolving data difficult.
  - Scaling out (horizontal scaling) is hard; vertical scaling is expensive.
  - Poor handling of semi‑structured/unstructured data (JSON, XML, documents).
- NoSQL advantages:
  - Elastic scaling across commodity servers.
  - High write throughput (eventual consistency).
  - Schema‑on‑read / dynamic schema.

## History of NoSQL Databases
- **1998** – Carlo Strozzi uses “NoSQL” for a lightweight relational DB (not the modern meaning).
- **2000s** – Big internet companies (Google, Amazon, Facebook) build custom stores:
  - Google Bigtable (2004), Amazon Dynamo (2007).
- **2009** – Term revived for distributed non‑relational stores; MongoDB, Cassandra, HBase emerge.

## Features of NoSQL
- **Schema‑less** – documents or key‑value pairs without predefined columns.
- **Horizontal scaling** – automatic sharding, replication.
- **Relaxed ACID** – often BASE (Basically Available, Soft state, Eventually consistent) instead of ACID.
- **Distributed by design** – fault‑tolerant, peer‑to‑peer.

## Types of NoSQL Databases

### 1. Key‑Value Stores
- Data = unique key → value (blob, JSON, string).
- Operations: `get(key)`, `put(key, value)`.
- Examples: **Redis** (in‑memory), **Riak**, **Amazon DynamoDB**, **Etcd**.
- Use cases: session caching, user profiles, shopping carts.

### 2. Document Stores
- Store semi‑structured documents (XML, JSON, BSON).
- Query by fields inside documents (secondary indexes).
- Examples: **MongoDB**, **Couchbase**, **Firestore**.
- Use cases: catalogs, logs, CMS.

### 3. Column‑Family Stores (Wide‑Column)
- Data stored in tables with rows and dynamic columns grouped into column families.
- Optimised for aggregation over many rows but few columns.
- Examples: **Apache Cassandra**, **HBase** (on Hadoop), **ScyllaDB**.
- Use cases: time‑series data, IoT, event logging.

### 4. Graph Databases
- Nodes (entities) and edges (relationships) with properties.
- Traversal queries are extremely fast (milliseconds for multi‑hop joins).
- Examples: **Neo4j**, **ArangoDB**, **Amazon Neptune**.
- Use cases: social networks, fraud detection, recommendation engines.

## Query Mechanism Tools for NoSQL
- **MongoDB** – query language similar to JSON (find, aggregate pipeline).
- **Redis** – commands (`GET`, `SET`, `HSET`, `LRANGE`).
- **Cassandra** – CQL (Cassandra Query Language) – looks like SQL but without joins or aggregations.
- **Neo4j** – Cypher (declarative pattern‑matching language).
- **DynamoDB** – PartiQL (SQL‑compatible) or API calls.

## CAP Theorem (Brewer’s Theorem)
- In a distributed data system, you can only achieve **two** of three guarantees:
  - **Consistency** – every read receives the most recent write (or error).
  - **Availability** – every request receives a non‑error response, without guarantee that it contains the latest write.
  - **Partition tolerance** – system continues to operate despite arbitrary message loss or network partition.

- Since network partitions are inevitable in large‑scale systems, you must choose between **CP** or **AP**.

### Examples
- **CP systems** – HBase, MongoDB (with strong consistency), Zookeeper.  
  (If partition occurs, may block writes to preserve consistency.)
- **AP systems** – Cassandra, DynamoDB (eventual consistency), CouchDB.  
  (Continue accepting writes but may return stale reads.)

### PACELC extension
- Adds trade‑off between **Latency** and **Consistency** even when there is **No Partition** (E = Else, L = Latency vs C = Consistency).

## Vector Databases

### What are they?
- Databases designed to store, index, and query **vector embeddings** (high‑dimensional vectors).
- Vectors come from machine learning models (word2vec, BERT, ResNet, CLIP) representing text, images, audio, etc.

### Why needed?
- Traditional databases do not support **similarity search** (cosine, Euclidean, dot product) efficiently on hundreds of dimensions.
- Vector databases use specialised **ANN** (Approximate Nearest Neighbour) indexes: HNSW, IVF, PQ.

### Examples
- **Milvus**, **Weaviate**, **Pinecone**, **Qdrant**, **pgvector** (PostgreSQL extension).

### Use Cases
- **Semantic search** – retrieve documents by meaning (e.g., “animals that fly” → bats, birds).
- **Recommender systems** – find items similar to a user’s embedding.
- **Image retrieval** – find visually similar pictures.
- **LLM memory** – store conversation history as vectors for RAG (Retrieval Augmented Generation).

### Operations
- Insert vectors (with optional metadata).
- Query: `k‑nearest neighbours (k‑NN)` to a given vector.
- Hybrid search – combine vector similarity with metadata filters (e.g., “show me images of cats similar to this one”).

### Example (pgvector)
```sql
CREATE EXTENSION vector;
CREATE TABLE items (id SERIAL, embedding VECTOR(384));
SELECT * FROM items ORDER BY embedding <=> '[0.1, 0.2, ...]' LIMIT 10;

