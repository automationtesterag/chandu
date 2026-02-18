# PySpark / Spark – Detailed Answers (21–35)

## 21. Difference between RDD, DataFrame, and Dataset?

**RDD (Resilient Distributed Dataset)**

* Low-level API
* No schema
* Slower than DataFrame

**DataFrame**

* Structured data with columns
* Optimized using Catalyst optimizer
* Most commonly used

**Dataset**

* Type-safe (mainly used in Scala/Java)
* Rarely used in Python

**Real-time example:**
In ETL pipelines, we usually use **DataFrames** because they are faster and easier to use.

**Interview tip:**
Say: *In real projects, I mostly use DataFrames because of performance and built-in optimizations.*

---

## 22. What are transformations and actions?

**Transformations**

* Operations that do not execute immediately
* Example: select(), filter(), join()

**Actions**

* Trigger execution
* Example: show(), count(), write()

**Real-time example:**

```
df.filter("age > 30")   → transformation
df.count()              → action
```

Spark runs the job only when an action is called.

---

## 23. Narrow vs Wide transformations

**Narrow Transformation**

* No data shuffle
* Faster

Examples:

* map()
* filter()

**Wide Transformation**

* Shuffle happens
* Slower

Examples:

* groupBy()
* join()

**Real-time example:**
Filtering sales by date = narrow
Grouping sales by region = wide

---

## 24. What causes shuffle?

Shuffle happens when:

* groupBy
* join
* orderBy
* distinct

**Why shuffle is slow**

* Data moves across nodes
* Disk and network usage

**Real-time example:**
Joining two large tables causes shuffle of large data.

---

## 25. What is lazy evaluation?

Spark does not execute immediately.
It builds a plan and runs only when an action is triggered.

**Real-time example:**
You write 10 transformations → Spark executes only when `.write()` or `.show()` is called.

**Benefit**

* Optimizes execution
* Removes unnecessary steps

---

## 26. Explain Spark execution flow

Simple flow:

1. User writes code
2. Driver creates logical plan
3. DAG created
4. Tasks sent to executors
5. Executors process data
6. Results returned

**Real-time example:**
When running a notebook, driver coordinates work and executors process partitions.

---

## 27. What is DAG in Spark?

DAG = Directed Acyclic Graph

It is the execution plan showing:

* transformations
* stages
* dependencies

**Real-time example:**
If you filter → join → groupBy
Spark builds DAG before executing.

---

## 28. What is broadcast join?

Broadcast join sends small table to all executors to avoid shuffle.

**When to use**

* One table small (<100MB usually)

**Real-time example:**
Customer table (5 MB)
Transactions table (100 GB)
Broadcast customer table.

```
broadcast(customer_df)
```

---

## 29. When to use repartition vs coalesce?

**repartition**

* Increase or decrease partitions
* Causes shuffle

**coalesce**

* Reduce partitions only
* No shuffle (faster)

**Real-time example:**
After filtering large data → reduce partitions using coalesce before writing.

---

## 30. How do you optimize joins?

Steps:

1. Broadcast small tables
2. Filter data before join
3. Use proper join keys
4. Repartition tables

**Real-time example:**
Filtering last 7 days data before join reduces processing time.

---

## 31. How do you handle skewed data?

Skew = uneven data distribution

Solutions:

* Salting technique
* Broadcast join
* Skew hints

**Real-time example:**
One region has 80% of sales → skew in join.

Add salt column to distribute data.

---

## 32. What is checkpointing?

Checkpointing saves intermediate data to storage.

**Why used**

* Break lineage
* Avoid recomputation
* Useful in streaming

**Real-time example:**
Long pipeline with many transformations → checkpoint improves reliability.

---

## 33. Explain memory management in Spark

Memory used for:

* Execution
* Storage (cache)

Spark divides memory between:

* Storage memory
* Execution memory

**Real-time example:**
If caching too much data → execution may fail due to memory shortage.

---

## 34. Difference between driver and executor?

**Driver**

* Controls job
* Creates DAG
* Sends tasks

**Executor**

* Executes tasks
* Processes partitions
* Returns results

**Real-time example:**
Notebook runs on driver
Data processing happens on executors

---

## 35. What are common performance tuning techniques in Spark?

Most practical points to say in interview:

1. Use DataFrames instead of RDD
2. Reduce shuffle operations
3. Broadcast small tables
4. Use partitioning properly
5. Avoid collect()
6. Use caching carefully
7. Optimize joins
8. Use Delta OPTIMIZE

**Real-time example:**
Broadcast join reduced pipeline runtime from 40 minutes to 15 minutes.

---

# Most Important Interview Tip (for Spark)

Interviewers often ask:

**"Why is your Spark job slow and how did you fix it?"**

Best structure to answer:

1. Problem
2. Root cause
3. Fix
4. Result

Example:
"Job was slow due to shuffle in join. I broadcasted the small table and reduced runtime by 50%."

---

# If you want to prepare smartly for interviews

Next, I can give you:

**Top 15 Spark performance tuning scenarios that interviewers ask very frequently (real production problems).**

Just say **"Spark scenarios"**.

Here are **real-world Spark / PySpark scenarios** that interviewers commonly ask for **5–8+ years experience**. I’ll keep answers **simple, practical, and interview-ready**.

---

# Spark / PySpark – Real Interview Scenarios

## 1. A Spark job is running very slow. How do you troubleshoot?

**Answer (structured way):**
I usually check:

1. Data size increase
2. Shuffle operations (joins, groupBy)
3. Number of partitions
4. Skewed data
5. Spark UI (Stages and Tasks)

**Real example:**
A job slowed because a new column increased dataset size, causing heavy shuffle during aggregation.

---

## 2. A join between two large tables is very slow. What will you do?

**Answer:**
Steps:

* Filter data before join
* Repartition tables on join key
* Broadcast smaller table if possible

**Real example:**
Filtering last 7 days of data before join reduced processing time by 60%.

---

## 3. Executor OutOfMemory error occurs. How do you fix it?

**Answer:**

* Increase executor memory
* Reduce partition size
* Avoid collect() or toPandas()
* Cache only necessary data

**Real example:**
Driver crashed because developer used `.collect()` on millions of rows.

---

## 4. Too many small files are created after writing data. How do you fix it?

**Answer:**

* Use coalesce() before write
* Use Delta OPTIMIZE

**Real example:**
Hourly jobs created thousands of small files → coalesce reduced file count.

---

## 5. Data skew is causing one task to run much longer than others. How do you fix it?

**Answer:**

* Salting technique
* Broadcast join
* Skew hints

**Real example:**
One customer ID had huge records → salting distributed load evenly.

---

## 6. Spark job fails randomly sometimes. What could be reasons?

**Answer:**

* Cluster autoscaling issues
* Source connectivity issues
* Memory pressure
* Storage throttling

**Real example:**
Pipeline failed at peak hours due to storage throttling.

---

## 7. How do you decide the number of partitions?

**Answer:**
General guideline:

* 2–3 partitions per CPU core
* Check partition size (100–200 MB recommended)

**Real example:**
Increasing partitions improved parallelism and reduced runtime.

---

## 8. How do you improve write performance to Data Lake?

**Answer:**

* Partition data properly
* Avoid writing single file
* Use parallel writes

**Real example:**
Partitioning by date improved downstream query speed.

---

## 9. A stage has one task running much longer than others. What does it indicate?

**Answer:**
Usually indicates:

* Data skew
* Uneven partitioning

**Real example:**
One partition had 80% of records due to skewed key.

---

## 10. How do you debug a failed Spark job?

**Answer:**
Steps:

1. Check notebook error
2. Check Spark UI
3. Check executor logs
4. Identify failing stage

**Real example:**
Null pointer exception due to unexpected null values in input data.

---

## 11. Job runs fine in development but fails in production. Why?

**Answer:**
Common reasons:

* Larger data volume
* Different cluster size
* Different schema

**Real example:**
Dev had 1GB data; production had 500GB, causing memory issues.

---

## 12. How do you optimize groupBy operations?

**Answer:**

* Filter data first
* Repartition before groupBy
* Use approximate aggregations if possible

**Real example:**
Filtering unnecessary columns reduced shuffle.

---

## 13. How do you handle late arriving data?

**Answer:**

* Use merge/upsert logic
* Reprocess last few days of data

**Real example:**
Sales data sometimes arrives next day → pipeline reprocesses last 2 days.

---

## 14. How do you monitor Spark jobs?

**Answer:**

* Spark UI
* Job logs
* Azure Monitor

**Real example:**
Monitoring showed one stage taking most of the runtime.

---

## 15. Most important scenario (very common)

**Tell me about a performance issue you solved in Spark.**

**Best way to answer:**

Structure:

1. Problem
2. Root cause
3. Fix
4. Result

**Example answer:**
"A join operation was slow due to shuffle of large tables. I broadcasted the small table and reduced runtime from 45 minutes to 18 minutes."

---

# What Interviewers Actually Check

When asking Spark scenarios, interviewers mainly evaluate:

* Do you understand shuffle?
* Do you know Spark UI?
* Can you troubleshoot performance?
* Have you handled real production issues?

If you answer with:

* problem
* analysis
* fix
* impact

you sound like an experienced engineer.

---


