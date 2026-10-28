1. What is Databricks, and how does it differ from standard Apache Spark?
- Answer: Databricks is a cloud-based unified data analytics platform built on top of Apache Spark. While Apache Spark is an open-source, distributed cluster-computing framework used for processing large datasets, Databricks is a fully managed platform that wraps around Spark.
- Key Differences:
  - Management: Databricks provides an out-of-the-box collaborative workspace, automated cluster management, and built-in security features, whereas standard Spark requires manual setup and infrastructure configuration.
  - Optimizations: Databricks includes proprietary components like the Databricks Runtime (DBR) and the Photon engine (a vectorized query engine written in C++) which offer significantly faster performance than vanilla Spark.

2. What is Delta Lake, and why is it preferred over raw formats like Parquet?
- Answer: Delta Lake is an open-source storage layer that brings reliability, security, and performance to data lakes. It sits on top of your existing cloud storage (like AWS S3, Azure Blob/ADLS Gen2, or Google Cloud Storage) as a collection of Parquet files.
- Key Advantages over Parquet:
  - ACID Transactions: Ensures data integrity so that reads and writes never result in corrupted or half-written data.
  - Time Travel: Allows you to query, audit, or roll back to previous versions of data using timestamps or version numbers.
  - Schema Enforcement & Evolution: Automatically rejects writes that don't match the table schema or safely allows structural changes (like adding new columns).

3. What is the difference between an All-Purpose Cluster and a Job Cluster?
- Answer: Databricks offers two types of compute clusters depending on your workload needs:
  - All-Purpose (Interactive) Clusters: Used for interactive development, exploratory data analysis, and running collaborative notebooks. They stay running until manually terminated or timed out due to inactivity, and multiple users can share them.
  - Job Clusters: Created automatically by the Databricks job scheduler to execute specific, automated background tasks (like an ETL pipeline). They spin up when the job starts and terminate automatically once the job finishes, making them cost-effective for production workloads.

4. What is the Medallion Architecture in Databricks?
- Answer: The Medallion Architecture is a design pattern used to logically organize data in a lakehouse, structuring it across three incremental quality layers:
  - Bronze Layer (Raw): Ingests raw data directly from source systems (JSON, CSV, Kafka streams) in its native format, maintaining history and a replica of source data.
  - Silver Layer (Cleaned & Conformed): Data is cleansed, filtered, deduplicated, and matched. Tables here provide an enterprise-view of cleaned data ready for ad-hoc analysis.
  - Gold Layer (Aggregated / Business-Level): Highly refined and aggregated data structured specifically to drive business reporting, dashboards, machine learning, and downstream analytical applications.

5. What are "Magic Commands" in Databricks notebooks?
- Answer: Magic commands are language-specific directives placed at the top of a notebook cell that allow you to override the default language of the notebook. Databricks notebooks support Python, SQL, Scala, and R.
- Common Examples:
  - %sql — Allows you to run SQL queries inside a Python notebook cell.
  - %python — Switches the cell context back to Python.
  - %fs — Allows you to interact with the Databricks File System (DBFS) using shell-like commands (e.g., %fs ls /databricks-datasets).

6. What is ETL and how does it differ from ELT?
- Answer: ETL stands for Extract, Transform, Load, where transformation happens before data is loaded into the target warehouse. ELT stands for Extract, Load, Transform, where data is loaded into the target storage system first and transformed natively inside the platform.
- Key Differences:
  - Processing Engine: ETL uses a separate, external transformation engine, while ELT leverages the high-performance computing capabilities of modern target platforms (like cloud data warehouses or lakehouses).
  - Use Case Example: Traditional ETL tools for legacy databases versus loading raw data directly into a Databricks lakehouse (Bronze) and transforming it using PySpark and Delta Lake (Silver/Gold).

7. What is Unity Catalog in Databricks, and why is it important?
- Answer: Unity Catalog is a fine-grained governance solution for data and AI on the Databricks Lakehouse. It provides centralized access control, auditing, lineage, and discovery for all data assets across your cloud storage.
- Why it matters:
  - Centralized Security: Grants permissions down to the table, row, or column level using standard SQL (`GRANT` / `REVOKE`) across workspaces.
  - Automatic Lineage: Automatically captures end-to-end data lineage across notebooks, workflows, and tables.
  - Three-Level Namespace: Organizes data hierarchically using `catalog.schema.table`.

8. How does Delta Lake handle concurrent writes, and what isolation levels does it support?
- Answer: Delta Lake uses optimistic concurrency control (OCC). Before committing, it checks if any files read by the current transaction were modified by a concurrent transaction.
- Isolation Levels:
  - *Serializable* (default): Ensures complete isolation, preventing write conflicts or phantom reads.
  - *WriteSerializable*: Allows concurrent writes as long as they don't logically conflict on the same partitions or data files.

9. What happens during a `VACUUM` command in Delta Lake, and what are the risks?
- Answer: `VACUUM` removes obsolete Parquet data files that are no longer part of the current table state and are older than the retention threshold (default 7 days) to save storage costs.
- Risks: Reducing the retention window too aggressively and running `VACUUM` permanently breaks **Time Travel** for snapshots older than that threshold and can cause active long-running concurrent jobs to fail if they reference just-deleted files.

10. What is the difference between Partitioning and Z-Ordering?
- Answer:
  - **Partitioning:** Physically separates data into distinct directories based on low-cardinality columns (e.g., `date`, `country`). Best for broad, coarse-grained file filtering.
  - **Z-Ordering:** Multi-dimensionally clusters data *within* files based on high-cardinality columns (e.g., `user_id`, `call_id`) using min/max file statistics to maximize data-skipping performance.

11. What causes a Spark Shuffle, and how do you diagnose data skew?
- Answer: A shuffle occurs when Spark redistributes data across different executor nodes because rows need to be grouped or joined together (`groupBy()`, `join()`, `distinct()`).
- Diagnosing Data Skew: Look at the Spark UI task metrics for a stage where most tasks finish quickly while a single task takes significantly longer (long tail). Solutions include broadcast joins, salting keys, or custom partitioning.

12. What is the difference between `cache()` / `persist()` and `checkpoint()` in Spark?
- Answer:
  - **`cache()` / `persist()`:** Stores a DataFrame in memory/disk lazily but *retains the lineage graph*, meaning it recomputes data from source if an executor fails.
  - **`checkpoint()`:** Cuts off the lineage graph entirely by writing the DataFrame out to reliable cloud storage, preventing stack overflows during massive iterative graph or machine learning training loops.
