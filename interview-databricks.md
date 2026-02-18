# Azure Databricks – Detailed Answers (1–20)

## 1. What is Azure Databricks and where is it used?

**Answer:**

Azure Databricks is a **cloud-based big data processing platform** built on Apache Spark. It is used to **process large data, transform data, and build data pipelines**.

**Where it is used:**

* ETL pipelines
* Data lake processing
* Machine learning
* Reporting preparation

**Real-time example:**
A retail company stores daily sales files in **Azure Data Lake**.
Databricks processes those files, cleans data, aggregates sales, and loads it into a **Gold layer table** for Power BI.

---

## 2. Difference between Databricks and traditional Spark clusters?

**Answer:**

| Databricks            | Traditional Spark          |
| --------------------- | -------------------------- |
| Managed service       | Manual setup               |
| Easy UI and notebooks | CLI or custom tools        |
| Auto scaling          | Manual scaling             |
| Delta Lake support    | Needs manual configuration |

**Real-time example:**
Without Databricks, a team must install Spark, manage clusters, configure security.
With Databricks, cluster setup takes 2–3 minutes.

---

## 3. Explain the architecture of Azure Databricks

**Answer (Simple explanation):**

There are mainly two layers:

1. **Control Plane**

* Managed by Microsoft
* Notebook UI
* Cluster management

2. **Data Plane**

* Runs in your Azure subscription
* Stores and processes data

**Real-time example:**
Your data is always inside your Azure storage; Databricks only manages execution.

---

## 4. What are clusters in Databricks? Types?

**Answer:**

Cluster = Group of machines used to process data.

**Types:**

1. All-purpose cluster
2. Job cluster

**Real-time example:**
Developers use all-purpose clusters for development and testing.
Production pipelines use job clusters.

---

## 5. Difference between job cluster and all-purpose cluster?

| Job Cluster       | All Purpose Cluster |
| ----------------- | ------------------- |
| Created for a job | Always running      |
| Cheaper           | More expensive      |
| Production use    | Development use     |

**Real-time example:**
ADF triggers a job cluster → runs ETL → cluster shuts down automatically.

---

## 6. What is Delta Lake?

**Answer:**

Delta Lake is a **storage layer** on top of Data Lake that provides:

* ACID transactions
* Schema enforcement
* Time travel

**Real-time example:**
If a job fails while writing data, Delta prevents corrupted files.

---

## 7. Benefits of Delta Lake over Parquet?

| Parquet         | Delta              |
| --------------- | ------------------ |
| No transactions | ACID support       |
| No time travel  | Time travel        |
| Schema issues   | Schema enforcement |

**Real-time example:**
In Parquet, duplicate writes may corrupt tables.
Delta prevents this.

---

## 8. What is ACID compliance in Delta Lake?

**Answer:**

ACID means:

* Atomicity
* Consistency
* Isolation
* Durability

**Real-time example:**
If 1 million rows are written and failure happens, Delta either writes all rows or none.

---

## 9. Explain OPTIMIZE and ZORDER

**OPTIMIZE**

* Combines small files into larger files.

**ZORDER**

* Improves query speed by sorting data.

**Real-time example:**
Querying customer data by `customer_id` becomes faster after ZORDER.

---

## 10. What is Auto Loader?

**Answer:**

Auto Loader automatically detects new files in storage and processes them incrementally.

**Real-time example:**
Every hour new files come from an SFTP system. Auto Loader loads only new files.

---

## 11. How do you handle incremental loads in Databricks?

**Answer:**
Methods:

* Watermark column
* MERGE INTO
* Auto Loader

**Real-time example:**
Load only records where `last_updated_date > last_processed_date`.

---

## 12. What are notebooks and how do you modularize code?

**Answer:**

Notebook = Interactive environment to write code.

Modularization:

* Use `%run` command
* Use Python modules
* Create reusable functions

**Real-time example:**
One notebook for ingestion
One for transformation
One for loading

---

## 13. How do you schedule jobs in Databricks?

**Answer:**

Options:

* Databricks Workflows
* Azure Data Factory
* Cron schedules

**Real-time example:**
Daily pipeline at 2 AM using ADF trigger.

---

## 14. How do you handle large datasets performance tuning?

**Answer:**
Steps:

* Partition data
* Optimize joins
* Cache intermediate data
* Increase cluster size

**Real-time example:**
Repartition before join to reduce shuffle.

---

## 15. What is caching in Spark?

**Answer:**
Caching stores data in memory to avoid recomputation.

**Real-time example:**
A dataframe used in multiple transformations is cached to improve speed.

---

## 16. Difference between persist and cache?

| Cache           | Persist               |
| --------------- | --------------------- |
| Memory only     | Memory + disk options |
| Default storage | Custom storage levels |

**Real-time example:**
Persist used when memory is limited.

---

## 17. Explain partitioning in Spark

**Answer:**
Partitioning divides data into smaller chunks processed in parallel.

**Real-time example:**
1 TB data → divided into 200 partitions → faster processing.

---

## 18. How do you debug a failed Databricks job?

**Answer:**
Steps:

1. Check job run logs
2. Check Spark UI
3. Check executor errors
4. Check cluster logs

**Real-time example:**
Out of memory error → increase executor memory.

---

## 19. How do you connect Databricks with Azure Data Factory?

**Answer:**
Steps:

1. Create Linked Service
2. Provide workspace URL
3. Use Databricks activity

**Real-time example:**
ADF triggers notebook daily to process files.

---

## 20. How do you manage secrets in Databricks?

**Answer:**
Use:

* Azure Key Vault
* Databricks Secret Scope

**Real-time example:**
Store database password in Key Vault instead of notebook.

---

# Interview Tip (Very Important)

For **7+ years experience**, always answer like this structure:

1. Definition
2. Real-time use case
3. Optimization or best practice

That makes answers sound senior-level.

---

If you want, I can now give you:

**Top 20 Azure Databricks scenario questions that interviewers ask most frequently (real production problems).**

Just say **“Databricks scenarios”**.

Here are **real-world Azure Databricks scenario questions** that are commonly asked in interviews for **5–8+ years experience**, along with **simple explanations and practical answers** you can use.

---

# Azure Databricks – Real Interview Scenarios

## 1. A job that used to run in 20 minutes now takes 1 hour. What will you check?

**Answer (structured way to say in interview):**

I would check:

1. Data size increase
2. Shuffle operations (joins, groupBy)
3. Skewed partitions
4. Cluster size or configuration changes
5. Spark UI stages and tasks

**Real example:**
A sales aggregation job slowed because one region had much more data, causing skew during join.

---

## 2. You see too many small files in a Delta table. How do you fix it?

**Answer:**
Use:

```
OPTIMIZE table_name
```

**Why this happens:**
Frequent small writes or streaming jobs.

**Real example:**
Hourly ingestion created thousands of 1–2 MB files, slowing queries.

---

## 3. Join is taking very long. How do you optimize it?

**Answer:**
Steps:

* Broadcast small table
* Repartition large tables
* Filter data before join

**Real example:**
Joining customer table (1 MB) with transactions (100 GB) → broadcast customer table.

---

## 4. A Databricks notebook fails with OutOfMemory error. What do you do?

**Answer:**

* Increase executor memory
* Reduce partitions
* Avoid collect()
* Cache only required data

**Real example:**
Developer used `.collect()` on 10 million rows → driver crashed.

---

## 5. How do you process only new files from a folder?

**Answer:**
Use:

* Auto Loader
  or
* Maintain processed file log table

**Real example:**
Daily CSV files arrive from SFTP → Auto Loader processes only new files.

---

## 6. How do you handle duplicate records in a pipeline?

**Answer:**
Options:

* dropDuplicates()
* MERGE INTO with primary key

**Real example:**
API sometimes sends duplicate orders → merge into Delta table.

---

## 7. Data skew problem in Spark. How do you fix it?

**Answer:**

* Salting technique
* Broadcast join
* Skew hints

**Real example:**
90% transactions belonged to one country → skew in join.

---

## 8. A job fails randomly sometimes. How do you troubleshoot?

**Answer:**
Steps:

1. Check logs
2. Check cluster autoscaling
3. Check source availability
4. Retry logic

**Real example:**
Failure due to storage throttling during peak hours.

---

## 9. How do you design a production ETL pipeline in Databricks?

**Answer:**
Typical design:

1. Ingest raw files to Bronze
2. Clean data to Silver
3. Aggregation to Gold

**Real example:**
Retail pipeline using Medallion Architecture.

---

## 10. How do you handle schema changes in incoming files?

**Answer:**
Use:

```
mergeSchema = true
```

**Real example:**
New column added in JSON file → pipeline handled automatically.

---

## 11. Cluster startup time is slowing pipelines. What do you do?

**Answer:**

* Use job clusters efficiently
* Use cluster pools

**Real example:**
Cluster pool reduced startup time from 5 minutes to 40 seconds.

---

## 12. How do you reduce Databricks cost?

**Answer:**

* Use job clusters
* Auto termination
* Right-size clusters

**Real example:**
Switching all-purpose clusters to job clusters reduced cost by 40%.

---

## 13. How do you monitor Databricks jobs?

**Answer:**

* Jobs UI
* Azure Monitor
* Logs

**Real example:**
Email alert configured for failed jobs.

---

## 14. How do you securely store database credentials?

**Answer:**
Use:

* Azure Key Vault
* Secret Scope

**Real example:**
Password stored in Key Vault instead of notebook.

---

## 15. How do you handle late arriving data?

**Answer:**

* Reprocess window
* Merge logic

**Real example:**
Sales records sometimes arrive next day → pipeline merges data.

---

## 16. How do you improve query performance on large Delta tables?

**Answer:**

* OPTIMIZE
* ZORDER
* Partitioning

**Real example:**
ZORDER on customer_id improved queries by 60%.

---

## 17. Databricks job triggered from ADF fails. What will you check?

**Answer:**

* Notebook path
* Cluster configuration
* Linked service credentials

**Real example:**
ADF failed because cluster was terminated.

---

## 18. How do you design logging in Databricks pipelines?

**Answer:**
Create a log table:

* start time
* end time
* status
* records processed

**Real example:**
Logging helped identify slow transformation step.

---

## 19. How do you process large files (100GB+) efficiently?

**Answer:**

* Partitioning
* Parallel read
* Avoid single file writes

**Real example:**
Large JSON file split before processing.

---

## 20. Most important scenario (very common in interviews)

**Explain one pipeline you designed end-to-end.**

**Best structure to answer:**

1. Source
2. Ingestion
3. Transformation
4. Storage
5. Scheduling
6. Monitoring
7. Optimization

Interviewers mainly judge:

* clarity
* real experience
* troubleshooting thinking

---

# Most Frequently Asked Scenario (very important)

Almost every interviewer asks:

**"Tell me a performance issue you faced and how you fixed it."**

Prepare one strong real story:

* Problem
* Root cause
* Fix
* Result

This question alone often decides interviews.

---

