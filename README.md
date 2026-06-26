# Databricks Travel Booking SCD2 Data Warehouse Project <br/>

### 🚀 Tech Stack
**Compute & Processing:** Databricks, PySpark, Delta Lake  
**Governance & Security:** Unity Catalog, Databricks Volumes  
**Data Quality Framework:** PyDeequ (Amazon Deequ for Spark)  
**Orchestration:** Databricks Workflows (Multitask DAGs)  

---

### 📋 Core Architectural Features

* **Bronze Ingestion from Volumes**
    * Architected a robust, parameterized ingestion pipeline to pull raw `bookings` and `customers` datasets from **Databricks Volumes**.
    * Implemented mandatory metadata auditing columns (`ingested_at`, `source_file_name`) combined with **idempotent appends** to eliminate data duplication during pipeline retries.

* **Automated Data Quality (PyDeequ)**
    * Integrated the **PyDeequ** framework to enforce operational constraints including structural completeness, value validity, non-negativity, and primary key uniqueness.
    * Persisted all execution metrics and verification rules directly into a centralized metadata logging table (`ops.dq_results`) for continuous pipeline observability.

* **SCD Type 2 Customer Dimension**
    * Implemented a comprehensive **Slowly Changing Dimension (SCD) Type 2** architecture for customer profiles.
    * Managed historical state tracking utilizing system-generated surrogate keys, `effective_date`, `expiry_date` boundaries, and active status indicators (`is_current`) via an automated Delta Lake `MERGE` close-and-insert engine.

* **Enriched Booking Fact Table**
    * Designed a transaction-grain `booking_fact` table optimized for downstream BI tools.
    * Engineered a pipeline to map incoming natural keys to active dimension surrogate keys, aggregate metrics (`total_amount`, `total_quantity`) at a daily grain, and execute safe, deterministic upserts using `MERGE`.

* **Lakehouse Performance Optimization & Operations**
    * Configured automated storage management scripts utilizing **Z-ORDER multidimensional clustering** on frequent join/filter predicates.
    * Controlled storage cost overheads via **VACUUM file retention cleaning** and enforced optimal Cost-Based Optimizer (CBO) query paths using **ANALYZE TABLE** statistical computations.

* **Idempotent Analytics SQL Layer**
    * Developed decoupled aggregation scripts to build a holistic **Customer 360 view** and track core business KPIs like daily revenue patterns.
    * Enforced data integrity across reporting layers by deploying an idempotent **delete-then-insert execution pattern** for backfills.

* **End-to-End Workflow Orchestration**
    * Configured a modular task graph (DAG) within **Databricks Workflows** linking dependent engineering notebooks directly to final production SQL scripts.
    * Parameterized the entire execution tree using dynamic `arrival_date` widgets to enable flawless historical backfills and scheduled batch runs.
<br/>
## Pyspark Notebooks Data Pipeline <br/>
<img width="2356" height="436" alt="image" src="https://github.com/user-attachments/assets/73123b8f-fdc9-47b3-82b6-88661de4937f" />
<br/>
## SQL Data Pipeline <br/>
<img width="1874" height="818" alt="image" src="https://github.com/user-attachments/assets/3cd88273-c33e-47c1-912b-f87b035306cb" />
