# Understanding LSM Trees, B+ Trees & Hybrid Indexing Models

- [Understanding LSM Trees, B+ Trees \& Hybrid Indexing Models](#understanding-lsm-trees-b-trees--hybrid-indexing-models)
  - [1. LSM Trees (Log-Structured Merge Trees)](#1-lsm-trees-log-structured-merge-trees)
    - [How It Works?](#how-it-works)
    - [Pros \& Cons](#pros--cons)
    - [Example Databases Using LSM Trees](#example-databases-using-lsm-trees)
  - [2. B+ Trees (Balanced Tree Indexing)](#2-b-trees-balanced-tree-indexing)
    - [How It Works?](#how-it-works-1)
    - [Pros \& Cons](#pros--cons-1)
    - [Example Databases Using B+ Trees](#example-databases-using-b-trees)
  - [3. Hybrid Indexing Models (Combining LSM \& B+ Trees)](#3-hybrid-indexing-models-combining-lsm--b-trees)
    - [A. LSM-B+ Hybrid (MongoDB's WiredTiger, MyRocks)](#a-lsm-b-hybrid-mongodbs-wiredtiger-myrocks)
    - [B. Fractal Trees (TokuDB, Percona FT)](#b-fractal-trees-tokudb-percona-ft)
    - [C. Bw-Trees (Hekaton in SQL Server, FAWN)](#c-bw-trees-hekaton-in-sql-server-fawn)
  - [4. Which Indexing Model is Best for Mixed Read \& Write Workloads?](#4-which-indexing-model-is-best-for-mixed-read--write-workloads)
  - [5. Conclusion](#5-conclusion)

## 1. LSM Trees (Log-Structured Merge Trees)

### How It Works?

- **Write Optimization:** Writes are buffered in memory (MemTable) and later flushed to immutable SSTables (Sorted String Tables).
- **Compaction:** Periodically merges multiple SSTables to reduce query latency.
- **Read Path:** Queries must merge results from multiple SSTables, which can increase read latency.

### Pros & Cons

| Advantage | Disadvantage |
|-----------|-------------|
| High write throughput (sequential writes) | Read latency increases for older data |
| Efficient for append-only workloads | Compaction requires CPU and disk I/O |
| Works well for time-series data | Querying multiple SSTables can slow down complex reads |

### Example Databases Using LSM Trees

- **Cassandra** (Optimized for high write throughput, eventual consistency)
- **RocksDB** (Embedded key-value store, used in MyRocks for MySQL)
- **Apache Druid** (OLAP system with real-time ingestion and LSM-like storage)

---

## 2. B+ Trees (Balanced Tree Indexing)

### How It Works?

- **Write Path:** Inserts and updates occur in-place, maintaining a balanced tree structure.
- **Read Path:** Fast lookups (O(log N)) as data is already sorted and indexed.
- **Uses disk pages efficiently** with node splits happening as needed.

### Pros & Cons

| Advantage | Disadvantage |
|-----------|-------------|
| Efficient range queries | Writes require random I/O (node splits) |
| Good for transactional workloads (OLTP) | Slower than LSM for insert-heavy workloads |
| Fast reads for all data, including historical | Index maintenance can become expensive |

### Example Databases Using B+ Trees

- **MySQL (InnoDB)** (Optimized for fast indexed lookups)
- **PostgreSQL** (Uses B+ Trees for indexing and metadata)
- **TimescaleDB** (B+ Trees + BRIN for efficient time-series queries)

---

## 3. Hybrid Indexing Models (Combining LSM & B+ Trees)

### A. LSM-B+ Hybrid (MongoDB's WiredTiger, MyRocks)

- **Write Path:** Uses LSM for batched sequential writes.
- **Read Path:** Uses B+ Trees for efficient indexing and query lookups.
- **Compaction is optimized** to avoid high read latency.

**Example Databases:**

- **MongoDB (WiredTiger)** - Balances write performance with fast indexed reads.
- **MyRocks (MySQL Engine)** - Uses RocksDB (LSM-based) for optimized writes while keeping relational query power.

### B. Fractal Trees (TokuDB, Percona FT)

- **Write Path:** Uses buffered writes inside tree nodes to optimize disk I/O.
- **Read Path:** Reads are faster than LSM since fewer SSTables need merging.

**Example Databases:**

- **TokuDB (MySQL Engine by Percona)** - High insertion speed, good for analytical workloads.
- **Percona FT** - Uses fractal tree indexing to improve both write and read performance.

### C. Bw-Trees (Hekaton in SQL Server, FAWN)

- **Write Path:** Lock-free, copy-on-write tree structure.
- **Read Path:** Versioned read optimizations for multi-threaded access.

**Example Databases:**

- **Hekaton (SQL Server In-Memory OLTP Engine)** - Optimized for high concurrency.
- **FAWN (Fast Array of Wimpy Nodes)** - Efficient distributed storage for key-value workloads.

---

## 4. Which Indexing Model is Best for Mixed Read & Write Workloads?

| Indexing Model | Write Performance | Read Performance | Best Used In |
|---------------|----------------|----------------|-------------|
| **B+ Tree** | ❌ Slow (random writes) | ✅ Fast (O(log N), great for range queries) | OLTP Databases (MySQL, PostgreSQL) |
| **LSM Tree** | ✅ Fast (sequential writes) | ❌ Slower for historical reads | Write-heavy workloads (Cassandra, RocksDB) |
| **Fractal Tree** | ✅✅ Very Fast (buffered writes) | ✅ Fast (optimized range queries) | Analytical & mixed workloads (TokuDB) |
| **Bw-Tree** | ✅ Fast (lock-free writes) | ✅✅ Very Fast (copy-on-write reads) | High concurrency OLTP (SQL Server Hekaton) |
| **Hybrid (LSM + B+ Tree)** | ✅ Fast (batched writes) | ✅ Fast (optimized index lookups) | MongoDB (WiredTiger), MyRocks |

---

## 5. Conclusion

- **If you are optimizing for write-heavy workloads:** → Use **LSM Tree-based databases** (Cassandra, Druid, RocksDB).
- **If you need fast transactional reads & writes:** → Use **Bw-Trees (Hekaton) or Fractal Trees (TokuDB, Percona FT).**
- **If you need a balance between SQL and high-performance indexing:** → Use **Hybrid models like MongoDB (WiredTiger) or MyRocks.**

---
