# Database Comparison

## Comparison of Database Loads

- PostgreSQL
- DynamoDB
- Cassandra
- MongoDB

| `Criteria`                   | `PostgreSQL`                        | `DynamoDB`                              | `Cassandra`                           | `MongoDB`                             |
|---------------------------------|---------------------------------------|-------------------------------------------|-----------------------------------------|-----------------------------------------|
| `Data Model`                  | Relational (SQL)                      | Key-Value / Document                      | Wide-Column (Keyspace/Table)            | Document (BSON)                         |
| `Consistency Model`           | Strong Consistency (ACID)             | Eventual or Strong (Configurable)         | Eventual Consistency (Tunable)          | Eventual or Strong (Configurable)       |
| `Write Load Handling`         | Moderate to High                      | Very High                                | Very High                              | High                                    |
| `Write Throughput`            | ~500 to 5,000 writes per second       | 50,000+ WPS, can scale to millions        | 50,000+ WPS, can scale to millions      | ~5,000 to 50,000 writes per second      |
| `Read Load Handling`          | Moderate to High                      | Very High                                | Very High                              | High                                    |
| `Read Throughput`             | ~1,000 to 10,000 queries per second   | 100,000+ QPS, can scale to millions       | 100,000+ QPS, can scale to millions     | ~10,000 to 100,000 queries per second   |
| `Scalability`                 | Vertical Scaling (some Horizontal)    | Horizontal (Auto-scaling, Serverless)     | Horizontal (Manual Sharding, Scaling)   | Horizontal (Sharding, Replica Sets)     |
| `Latency`                     | Low to Moderate (10-100 ms)           | Low Latency (Single-digit milliseconds)   | Low Latency (2-10 ms)                  | Low to Moderate Latency (5-50 ms)       |
| `Query Complexity`            | High (SQL, Complex Joins, ACID)       | Low (Simple Queries, Key-based Access)    | Moderate (No Joins, Focus on Partition)| High (Rich Query Language, Aggregation) |
| `Use Case Focus`              | Transactional Workloads, Analytics    | Key-Value Operations, High Throughput     | Write-Intensive, Time-Series, Logging  | Flexible Schema, Semi-Structured Data   |

### Quantified Load Summary Table

| `Database`    | `Write Load Handling`            | `Read Load Handling`             | `Latency`                             | `Best Use Cases`                              |
|-----------------|------------------------------------|------------------------------------|-----------------------------------------|-------------------------------------------------|
| `PostgreSQL`   | Moderate to High (500-5,000 WPS)  | Moderate to High (1,000-10,000 QPS)| Low to Moderate (10-100 ms)             | Transactional systems, complex queries, analytics|
| `DynamoDB`     | Very High (50,000+ WPS, scalable) | Very High (100,000+ QPS, scalable) | Low (Single-digit ms)                   | High-throughput web apps, IoT, serverless apps   |
| `Cassandra`    | Very High (50,000+ WPS, scalable) | Very High (100,000+ QPS, scalable) | Low (2-10 ms)                           | Real-time data processing, time-series, logging  |
| `MongoDB`      | High (5,000-50,000 WPS)           | High (10,000-100,000 QPS)          | Low to Moderate (5-50 ms)               | Flexible schemas, semi-structured data, IoT      |

### Key Takeaways

- `PostgreSQL`: Moderate to high throughput with low to moderate latency, ideal for ACID-compliant, relational workloads.
- `DynamoDB`: Exceptional for extreme write and read loads, with low latency and auto-scaling, making it perfect for serverless, real-time applications.
- `Cassandra`: High write and read loads with low latency, designed for distributed systems needing high availability and large-scale data handling.
- `MongoDB`: High throughput with moderate scalability, well-suited for flexible, document-based data models requiring rich queries.

This breakdown helps in deciding the right database based on quantified load and latency needs for various system design scenarios.
