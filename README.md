5 basic Databricks interview questions:

1. What is Databricks, and how does it differ from standard Apache Spark?
 * Answer: Databricks is a cloud-based unified data analytics platform built on top of Apache Spark. While Apache Spark is an open-source, distributed cluster-computing framework used for processing large datasets, Databricks is a fully managed platform that wraps around Spark.
 * Key Differences:
   * Management: Databricks provides an out-of-the-box collaborative workspace, automated cluster management, and built-in security features, whereas standard Spark requires manual setup and infrastructure configuration.
   * Optimizations: Databricks includes proprietary components like the Databricks Runtime (DBR) and the Photon engine (a vectorized query engine written in C++) which offer significantly faster performance than vanilla Spark.
2. What is Delta Lake, and why is it preferred over raw formats like Parquet?
 * Answer: Delta Lake is an open-source storage layer that brings reliability, security, and performance to data lakes. It sits on top of your existing cloud storage (like AWS S3, Azure Blob/ADLS Gen2, or Google Cloud Storage) as a collection of Parquet files.
 * Key Advantages over Parquet:
   * ACID Transactions: Ensures data integrity so that reads and writes never result in corrupted or half-written data.
   * Time Travel: Allows you to query, audit, or roll back to previous versions of data using timestamps or version numbers.
   * Schema Enforcement & Evolution: Automatically rejects writes that don't match the table schema or safely allows structural changes (like adding new columns).
3. What is the difference between an All-Purpose Cluster and a Job Cluster?
 * Answer: Databricks offers two types of compute clusters depending on your workload needs:
   * All-Purpose (Interactive) Clusters: Used for interactive development, exploratory data analysis, and running collaborative notebooks. They stay running until manually terminated or timed out due to inactivity, and multiple users can share them.
   * Job Clusters: Created automatically by the Databricks job scheduler to execute specific, automated background tasks (like an ETL pipeline). They spin up when the job starts and terminate automatically once the job finishes, making them cost-effective for production workloads.
4. What is the Medallion Architecture in Databricks?
 * Answer: The Medallion Architecture is a design pattern used to logically organize data in a lakehouse, structuring it across three incremental quality layers:
   * Bronze Layer (Raw): Ingests raw data directly from source systems (JSON, CSV, Kafka streams) in its native format, maintaining history and a replica of source data.
   * Silver Layer (Cleaned & Conformed): Data is cleansed, filtered, deduplicated, and matched. Tables here provide an enterprise-view of cleaned data ready for ad-hoc analysis.
   * Gold Layer (Aggregated / Business-Level): Highly refined and aggregated data structured specifically to drive business reporting, dashboards, machine learning, and downstream analytical applications.
5. What are "Magic Commands" in Databricks notebooks?
 * Answer: Magic commands are language-specific directives placed at the top of a notebook cell that allow you to override the default language of the notebook. Databricks notebooks support Python, SQL, Scala, and R.
 * Common Examples:
   * %sql — Allows you to run SQL queries inside a Python notebook cell.
   * %python — Switches the cell context back to Python.
   * %fs — Allows you to interact with the Databricks File System (DBFS) using shell-like commands (e.g., %fs ls /databricks-datasets).
Data Engineer Practice
