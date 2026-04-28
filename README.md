# 🏥 Healthcare Advanced Analytics Pipeline (Snowflake)

## 🚀 Overview

This project implements a **production-grade, end-to-end data engineering pipeline** for healthcare analytics using Snowflake.

It follows a **multi-layered architecture** to transform raw healthcare data into **business-ready insights and KPIs**, with built-in **data quality checks, anomaly detection, and real-time processing**.

---

## 🎯 Problem Statement

Healthcare systems generate large volumes of fragmented data across:

* Patient records
* Appointments
* Medical diagnostics
* Billing systems

### Key Challenges

* Poor data quality and inconsistencies
* Lack of unified patient view
* No anomaly detection mechanism
* Limited operational and financial insights

### Solution

This project builds a **scalable Snowflake data platform** that enables:

* Patient 360 view
* Risk prediction analytics
* Revenue trend monitoring
* Operational efficiency tracking
* Data governance and anomaly detection

---

## 🏗️ Architecture

```
                ┌──────────────────────────┐
                │  External Stage (CSV)    │
                └────────────┬─────────────┘
                             │
                        COPY INTO
                             │
        ┌────────────────────▼────────────────────┐
        │                 RAW LAYER               │
        │   (Unprocessed, ingestion tables)      │
        └────────────────────┬────────────────────┘
                             │ Streams (CDC)
                             ▼
        ┌────────────────────────────────────────┐
        │            VALIDATED LAYER             │
        │  Data Cleaning + Data Quality Checks   │
        └────────────────────┬───────────────────┘
                             │
                             ▼
        ┌────────────────────────────────────────┐
        │             CURATED LAYER              │
        │   Dimensional Model (Facts & SCD-2)    │
        └────────────────────┬───────────────────┘
                             │
                             ▼
        ┌────────────────────────────────────────┐
        │            DATAMART LAYER              │
        │     Business-Level Aggregations        │
        └────────────────────┬───────────────────┘
                             │
                             ▼
        ┌────────────────────────────────────────┐
        │          ANALYTICAL LAYER              │
        │         KPIs & Insights               │
        └────────────────────────────────────────┘

                 ┌──────────────────────────────┐
                 │        GOVERNANCE LAYER      │
                 │  DQ Logs + Anomaly Engine    │
                 └──────────────────────────────┘
```

---

## 🧰 Tech Stack

* **Snowflake** – Cloud Data Warehouse
* **SQL** – Data transformations (ELT)
* **Snowflake Streams** – Change Data Capture (CDC)
* **Snowflake Tasks** – Pipeline orchestration
* **GitHub** – Version control

---

## 📥 Data Ingestion (RAW Layer)

### Purpose

* Store raw, unprocessed data
* Maintain ingestion traceability
* Enable replay capability

### Key Features

* External stage ingestion (CSV)
* COPY INTO with error handling
* Timestamp tracking

### Tables

* `raw_patients`
* `raw_appointments`
* `raw_medical_records`
* `raw_billing`

---

## ✅ Data Validation (VALIDATED Layer)

### Purpose

* Clean and standardize data
* Enforce business rules
* Capture bad records

### Key Features

* `TRY_CAST` for safe conversions
* Data Quality (DQ) rules
* Exception logging

### Examples of Rules

* Missing mandatory fields (gender, city)
* Invalid appointment status
* Non-numeric medical values
* Negative billing amounts

### Output Tables

* `valid_patients`
* `valid_appointments`
* `valid_medical_records`
* `valid_billing`

---

## 🛡️ Governance Layer

### Purpose

Ensure **data reliability and observability**

### Components

#### 1. DQ Exception Log

Captures invalid records with:

* Error type
* Business key
* Error message

#### 2. Anomaly Detection Engine

Detects:

* Double-booked patients
* Billing outliers (> 3 standard deviations)

---

## 🧱 Curated Layer (Dimensional Modeling)

### Purpose

Transform validated data into **analytics-ready models**

### Design Pattern

* Star Schema
* Fact + Dimension tables
* Surrogate keys

### Tables

#### Dimension

* `dim_patient` (SCD Type 2)

#### Facts

* `fact_appointments`
* `fact_medical_records`
* `fact_billing`

### Key Features

* Historical tracking (SCD Type 2)
* Surrogate key generation (`UUID`)
* Optimized joins for analytics

---

## 🔄 Pipeline Orchestration

### Automated using Snowflake Tasks

| Task                    | Description            |
| ----------------------- | ---------------------- |
| `task_process_dq`       | Runs validation layer  |
| `task_populate_curated` | Builds curated models  |
| `task_anomaly_engine`   | Runs anomaly detection |

### Execution Flow

```
Streams → Validation → Curated → Anomaly Detection
```

---

## 📊 Datamart Layer

### Purpose

Provide **business-ready datasets**

### Views

#### 1. Patient 360

* Total appointments
* Medical history
* Lifetime billing

#### 2. Risk Prediction

* Age-based risk scoring
* Cholesterol & BP analysis

#### 3. Revenue Trends

* Monthly revenue
* Department-level insights

#### 4. No-Show Analysis

* Missed appointments
* Department efficiency

---

## 📈 Analytical Layer (KPIs)

### Key Metrics

#### 1. Health Risk Ratio

* % of high-risk patients

#### 2. Top Departments

* Revenue performance

#### 3. Operational Inefficiency

* No-show rate

#### 4. Data Quality Score

* Total DQ issues tracked

---

## ⚙️ How to Run the Project

### Step 1: Setup Environment

```sql
CREATE DATABASE HEALTHCARE_ADVANCED_DB;
CREATE WAREHOUSE HEALTHCARE_WH;
```

### Step 2: Create Schemas

* raw
* validated
* curated
* governance
* datamart
* analytical

### Step 3: Upload Data

Upload CSV files to:

```
@external_stages.healthcare_stage
```

### Step 4: Load Data

Run:

```sql
COPY INTO raw tables
```

### Step 5: Run Processing

```sql
CALL validated.process_data_quality();
CALL curated.populate_curated_models();
CALL governance.run_anomaly_engine();
```

### Step 6: Enable Automation

```sql
ALTER TASK … RESUME;
```

### Step 7: Query KPIs

```sql
SELECT * FROM analytical.kpi_health_risk;
SELECT * FROM analytical.kpi_top_departments;
SELECT * FROM analytical.kpi_operational_inefficiency;
SELECT * FROM analytical.kpi_data_quality;
```

---

## 🚀 Key Features

* Real-time pipeline using Streams
* Automated workflows with Tasks
* Data Quality Framework with logging
* SCD Type 2 implementation
* Anomaly detection engine
* Patient 360 analytics
* End-to-end layered architecture

---

## 🔔 Monitoring & Alerts

* Snowflake Alerts configured
* Email notifications on anomaly detection
* Proactive governance monitoring

---

## 🔮 Future Enhancements

* Integrate **DBT** for transformation management
* Add **Machine Learning models** for risk prediction
* Build dashboards using **Power BI / Tableau**
* Implement **Airflow orchestration**

---

## 📌 Conclusion

This project demonstrates a **production-ready healthcare data platform** with:

* Scalable architecture
* Strong data governance
* Real-time processing
* Business-driven analytics

It bridges the gap between **raw data and decision intelligence**.

---
