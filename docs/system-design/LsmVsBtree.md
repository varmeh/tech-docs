# Understanding LSM Trees, B+ Trees & Hybrid Indexing Models

- [Understanding LSM Trees, B+ Trees \& Hybrid Indexing Models](#understanding-lsm-trees-b-trees--hybrid-indexing-models)
  - [1. LSM Trees (Log-Structured Merge Trees) vs. B+ Trees (Balanced Tree Indexing)](#1-lsm-trees-log-structured-merge-trees-vs-b-trees-balanced-tree-indexing)
    - [B+ Trees-Based Databases (e.g., MySQL/InnoDB, PostgreSQL, TimescaleDB)](#b-trees-based-databases-eg-mysqlinnodb-postgresql-timescaledb)
    - [LSM Trees-Based Databases (e.g., Cassandra, RocksDB, Apache Druid)](#lsm-trees-based-databases-eg-cassandra-rocksdb-apache-druid)
  - [2. Hybrid Indexing Models (Combining LSM \& B+ Trees)](#2-hybrid-indexing-models-combining-lsm--b-trees)
    - [A. LSM-B+ Hybrid (MongoDB's WiredTiger, MyRocks)](#a-lsm-b-hybrid-mongodbs-wiredtiger-myrocks)
    - [B. Fractal Trees (TokuDB, Percona FT)](#b-fractal-trees-tokudb-percona-ft)
    - [C. Bw-Trees (Hekaton in SQL Server, FAWN)](#c-bw-trees-hekaton-in-sql-server-fawn)
  - [3. Which Indexing Model is Best for Mixed Read \& Write Workloads?](#3-which-indexing-model-is-best-for-mixed-read--write-workloads)
  - [4. Conclusion](#4-conclusion)

## 1. LSM Trees (Log-Structured Merge Trees) vs. B+ Trees (Balanced Tree Indexing)

### B+ Trees-Based Databases (e.g., MySQL/InnoDB, PostgreSQL, TimescaleDB)

✅ **Efficient Reads for Older Data**

- B+ Trees keep data **sorted** and maintain **balanced levels** for fast lookups.
- Since **all index nodes** (except leaves) are stored in memory, traversing to a leaf node is **fast (O(log N))**.
- Reads are efficient even for **older data** since it’s already in a well-structured, sorted format.

❌ **Low Write Throughput**

- B+ Trees require **in-place updates** and **node splits** when inserting.
- This leads to **random I/O**, which is expensive for disk-based storage.
- As data grows, maintaining balance (split/merge operations) slows down **write performance**.

🔥 **Ideal Use Case:**

- **Transactional Databases** (OLTP) with **frequent reads & updates**.
- **Efficient range queries** (since data is sorted).
- **Older data remains equally fast to query**.

---

### LSM Trees-Based Databases (e.g., Cassandra, RocksDB, Apache Druid)

✅ **Optimized for Writes**

- **Log-structured merges (LSM) buffer writes** in **MemTables** (in-memory structures).
- When MemTables reach a threshold, they are **flushed as immutable SSTables** (Sorted String Tables).
- This results in **sequential writes**, which are much faster than random I/O.

❌ **Read Latency Increases for Older Data**

- **Recent data** is in the **MemTable (fastest access)**.
- **Older data** is in **multiple SSTables**, requiring **merge operations** to reconstruct records.
- As data ages, more **SSTables must be scanned**, leading to **higher read latency**.
- **Compaction** merges SSTables periodically to improve query efficiency, but at a CPU/storage cost.

🔥 **Ideal Use Case:**

- **Write-heavy workloads** (e.g., logs, analytics, real-time event processing).  
- **Time-series & append-only data** where updates are rare.  
- **High ingestion rates** where write performance is crucial.  

---

## 2. Hybrid Indexing Models (Combining LSM & B+ Trees)

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

## 3. Which Indexing Model is Best for Mixed Read & Write Workloads?

| Indexing Model | Write Performance | Read Performance | Best Used In |
|---------------|----------------|----------------|-------------|
| **B+ Tree** | ❌ Slow (random writes) | ✅ Fast (O(log N), great for range queries) | OLTP Databases (MySQL, PostgreSQL) |
| **LSM Tree** | ✅ Fast (sequential writes) | ❌ Slower for historical reads | Write-heavy workloads (Cassandra, RocksDB) |
| **Fractal Tree** | ✅✅ Very Fast (buffered writes) | ✅ Fast (optimized range queries) | Analytical & mixed workloads (TokuDB) |
| **Bw-Tree** | ✅ Fast (lock-free writes) | ✅✅ Very Fast (copy-on-write reads) | High concurrency OLTP (SQL Server Hekaton) |
| **Hybrid (LSM + B+ Tree)** | ✅ Fast (batched writes) | ✅ Fast (optimized index lookups) | MongoDB (WiredTiger), MyRocks |

---

## 4. Conclusion

- **If you are optimizing for write-heavy workloads:** → Use **LSM Tree-based databases** (Cassandra, Druid, RocksDB).
- **If you need fast transactional reads & writes:** → Use **Bw-Trees (Hekaton) or Fractal Trees (TokuDB, Percona FT).**
- **If you need a balance between SQL and high-performance indexing:** → Use **Hybrid models like MongoDB (WiredTiger) or MyRocks.**

---

This document serves as a long-term reference for database indexing models. 🚀
