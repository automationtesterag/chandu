
# Azure Data Factory – Detailed Answers (36–55)

## 1. What is Azure Data Factory?

**Answer:**

Azure Data Factory (ADF) is a **cloud ETL/ELT service** used to **move and transform data** from different sources to destinations.

It is mainly used for:

* Data ingestion
* Data orchestration
* Scheduling pipelines

**Real-time example:**
Daily sales files arrive in Blob Storage → ADF copies them to Data Lake → triggers Databricks for transformation.

---

## 2. Components of ADF architecture?

**Main components:**

1. Pipeline – workflow of activities
2. Activities – tasks (Copy, Databricks, Data Flow)
3. Linked Services – connection to sources
4. Datasets – structure of data
5. Integration Runtime – execution engine
6. Triggers – scheduling pipelines

**Real-time example:**
Pipeline = daily ETL job
Activities = copy + notebook execution

---

## 3. What is a pipeline in ADF?

**Answer:**

A pipeline is a **logical group of activities** that perform a task.

**Real-time example:**
Pipeline steps:

1. Copy raw files
2. Run Databricks notebook
3. Load output to SQL

---

## 4. What are datasets and linked services?

**Linked Service**
Connection information (server, credentials).

**Dataset**
Represents data structure (table, file, folder).

**Real-time example:**
Linked Service = SQL Server connection
Dataset = Customer table

---

## 5. What is Integration Runtime?

**Answer:**

Integration Runtime (IR) is the **compute infrastructure used by ADF to move and transform data**.

It performs:

* Data movement
* Activity execution

---

## 6. Types of Integration Runtime?

1. Azure IR
2. Self-hosted IR
3. Azure SSIS IR

**Real-time example:**
On-prem database → Self-hosted IR required.

---

## 7. Self-hosted IR vs Azure IR?

| Self-hosted IR           | Azure IR       |
| ------------------------ | -------------- |
| Used for on-prem systems | Used for cloud |
| Installed on VM/server   | Fully managed  |

**Real-time example:**
Copy from on-prem Oracle → Self-hosted IR.

---

## 8. What are triggers in ADF?

Triggers start pipelines automatically.

Types:

* Schedule trigger
* Event trigger
* Tumbling window trigger

**Real-time example:**
Pipeline runs daily at 2 AM.

---

## 9. Difference between schedule trigger and event trigger?

**Schedule Trigger**
Runs at fixed time.

**Event Trigger**
Runs when file arrives.

**Real-time example:**
Daily batch → Schedule
File arrival → Event trigger

---

## 10. How do you implement incremental loads in ADF?

**Answer:**
Steps:

1. Maintain watermark value
2. Lookup last processed value
3. Filter source query

**Real-time example:**
Load records where:

```
LastModifiedDate > last_run_time
```

---

## 11. What is parameterization in pipelines?

**Answer:**
Using parameters makes pipelines reusable.

Examples:

* File path
* Table name
* Date

**Real-time example:**
Same pipeline loads multiple tables by passing table name parameter.

---

## 12. How do you handle failures in ADF pipelines?

**Answer:**
Methods:

* Retry logic
* Failure activity path
* Logging tables
* Alerts

**Real-time example:**
If copy fails, pipeline sends email alert.

---

## 13. What is retry logic?

**Answer:**
Retry automatically reruns failed activity.

**Real-time example:**
Temporary network issue → retry succeeds automatically.

---

## 14. How do you monitor pipelines?

**Answer:**
Monitoring options:

* Monitor tab in ADF
* Azure Monitor
* Log Analytics

**Real-time example:**
Check activity duration and failures in Monitor tab.

---

## 15. Difference between Copy Activity and Data Flow?

| Copy Activity | Data Flow          |
| ------------- | ------------------ |
| Moves data    | Transforms data    |
| Faster        | More compute heavy |

**Real-time example:**
Copy raw file → Copy Activity
Clean data → Data Flow or Databricks

---

## 16. What is Mapping Data Flow?

**Answer:**
Mapping Data Flow is a **graphical transformation tool** in ADF used for:

* Filtering
* Joining
* Aggregations

**Real-time example:**
Join two CSV files without writing code.

---

## 17. How do you pass parameters between pipelines?

**Answer:**
Use:

* Execute Pipeline activity
* Pipeline parameters

**Real-time example:**
Master pipeline calls child pipeline and passes date parameter.

---

## 18. How do you handle dynamic file names?

**Answer:**
Use dynamic expressions:

```
@concat('sales_',formatDateTime(utcNow(),'yyyyMMdd'),'.csv')
```

**Real-time example:**
Daily files named with date.

---

## 19. How do you connect ADF to Databricks?

**Answer:**
Steps:

1. Create Databricks Linked Service
2. Provide workspace URL and token
3. Use Notebook Activity

**Real-time example:**
ADF triggers notebook after file ingestion.

---

## 20. How do you optimize pipeline performance?

**Answer:**
Common steps:

1. Parallel activities
2. Partitioning in Copy Activity
3. Increase DIU
4. Use staging
5. Filter data early

**Real-time example:**
Parallel copy reduced load time from 2 hours to 40 minutes.

---

# Most Important ADF Interview Question (Very Common)

Interviewers often ask:

**"Explain one pipeline you designed in ADF."**

Best structure:

1. Source
2. Ingestion method
3. Transformation
4. Scheduling
5. Monitoring
6. Optimization

---


