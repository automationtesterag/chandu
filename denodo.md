# Denodo – Detailed Answers (56–70)

## 1. What is Denodo and why is it used?

**Answer:**

Denodo is a **data virtualization tool**. It allows you to **access and combine data from multiple sources without physically moving the data**.

**Why used:**

* Faster reporting
* Real-time data access
* Avoids heavy ETL jobs

**Real-time example:**
Customer data is in Oracle and order data is in SQL Server.
Denodo creates a virtual view combining both for reporting.

---

## 2. What is data virtualization?

**Answer:**

Data virtualization means **querying data from multiple sources in real time without storing it in a new location**.

**Real-time example:**
Instead of copying data to a warehouse daily, Denodo queries live databases when reports run.

---

## 3. Difference between ETL and data virtualization?

| ETL              | Data Virtualization |
| ---------------- | ------------------- |
| Moves data       | Does not move data  |
| Batch processing | Real-time queries   |
| Requires storage | No storage needed   |

**Real-time example:**
ETL: Load data nightly to warehouse
Denodo: Query source directly

---

## 4. Types of views in Denodo?

Main types:

1. Base View
2. Derived View
3. Interface View

**Real-time example:**
Base view → table from database
Derived view → join of two tables

---

## 5. What is base view and derived view?

**Base View**
Represents raw data from source.

**Derived View**
Transformation like:

* join
* filter
* aggregation

**Real-time example:**
Base view = Orders table
Derived view = Orders joined with Customers

---

## 6. What is caching in Denodo?

**Answer:**

Caching stores query results temporarily to improve performance.

**Real-time example:**
Slow API source → cache results → faster reporting.

---

## 7. Full cache vs partial cache?

**Full Cache**
Entire data stored in cache.

**Partial Cache**
Only frequently accessed data cached.

**Real-time example:**
Full cache for small tables
Partial cache for large transactional tables

---

## 8. How do you optimize Denodo queries?

**Answer:**
Steps:

* Enable query pushdown
* Use caching
* Reduce unnecessary joins
* Filter early

**Real-time example:**
Adding filter in derived view reduced query time from 2 minutes to 20 seconds.

---

## 9. What are data sources supported by Denodo?

Denodo supports:

* SQL databases
* APIs
* Excel/CSV
* Cloud storage
* NoSQL databases

**Real-time example:**
Sales from SQL Server + Customer data from REST API.

---

## 10. How does Denodo handle query pushdown?

**Answer:**
Denodo sends filters and joins to the source database whenever possible.

**Real-time example:**
If query filters on date, Denodo sends filter to database instead of fetching all data.

---

## 11. What is VQL?

**Answer:**

VQL (Virtual Query Language) is similar to SQL but used in Denodo to define views.

**Real-time example:**
Creating a derived view using VQL join.

---

## 12. What is Denodo scheduler?

**Answer:**

Denodo Scheduler is used to:

* Schedule cache refresh
* Run batch jobs
* Execute queries periodically

**Real-time example:**
Cache refresh every night at 1 AM.

---

## 13. How do you secure Denodo?

**Answer:**
Methods:

* Role-based access control
* LDAP/Active Directory
* Row-level security

**Real-time example:**
Finance users can see only finance tables.

---

## 14. How do you monitor Denodo performance?

**Answer:**
Tools:

* Execution trace
* Query monitor
* Logs

**Real-time example:**
Monitor shows which source query is slow.

---

## 15. How do you troubleshoot slow queries in Denodo?

**Answer:**
Steps:

1. Check execution plan
2. Check pushdown
3. Check source performance
4. Check cache

**Real-time example:**
Query slow because join was happening in Denodo instead of database → enabling pushdown fixed it.

---

# Most Important Denodo Interview Question (Very Common)

Interviewers often ask:

**"When should we use Denodo instead of ETL?"**

Best answer:

* Real-time reporting
* Multiple heterogeneous sources
* When latency of ETL is not acceptable

---

# If you want to be fully prepared (important)

For **7+ years experience**, one question is almost guaranteed:

**"Explain a real project where you used Denodo and why you chose virtualization instead of ETL."**

I can give you a **ready-to-use Denodo project explanation** that sounds real and impresses interviewers.

Just say **"Denodo project answer"**.
