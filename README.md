# 🛡️ Databricks-FinShield: End-to-End Hybrid Fraud Detection Platform

**Databricks-FinShield** is a modern, enterprise-grade data engineering platform designed for real-time credit card fraud detection. It seamlessly unifies low-latency streaming transactions with historical batch datasets on the **Databricks Lakehouse Platform**. 

By utilizing **Lakeflow Declarative Pipelines** and the **Delta Lake Medallion Architecture**, the system performs real-time stream-stream and stream-static joins to surface anomalous activities, power sub-second alerting systems, and feed executive analytical dashboards.

---
##  Architecture Overview

The system is built on a unified ingestion, processing, and consumption model governed comprehensively by **Unity Catalog**. 

```text
[Streaming: Kafka / Files]  ──► [Spark Streaming / Auto Loader] ──┐
                                                                 ├──► [Lakeflow Declarative Pipelines] ──► [Bronze ──► Silver ──► Gold] ──► [Alerts & Dashboards]
[Batch: Postgres SQL]       ──► [Lakeflow Connect]             ──┘
```


###  1. Ingestion Layer (Dual-Path)
* **Streaming Hot Path:** Ingests live transactional event streams from **Apache Kafka** and real-time JSON landfiles using **Spark Structured Streaming** and **Databricks Auto Loader** for optimal schema inference and incremental file processing.
* **Batch Cold Path:** Extracts historical customer profiles, merchant metadata, and legacy records from **Postgres SQL** databases utilizing **Lakeflow Connect** for reliable data replication.

###  2. Data Processing & Lakehouse Layer
Powered by **Lakeflow Pipelines (Apache Spark Declarative Pipelines)**, the core processing engine manages a structured multi-layer **Delta Lake** storage framework:
* **Bronze Layer:** Stores raw, unaltered data payloads from both ingestion paths to maintain an immutable data history.
* **Silver Layer:** Cleanses, standardizes schemas, and performs high-speed joins:
  * **Stream-Stream Joins:** Correlates fast-moving transaction events with concurrent interaction streams.
  * **Stream-Static Joins:** Enriches real-time transactional streams with historical, stationary customer profiles.
* **Gold Layer & Fraud Detection:** Runs localized fraud-scoring logic and encapsulates highly aggregated metrics optimized for consumption.

###  3. Consumption & Orchestration Layer
* **Realtime Alerts:** Triggers instant notifications for flagged fraudulent transactions using sub-second messaging patterns.
* **BI Dashboards:** Powers low-latency relational views for risk operations and executive monitoring.
* **Orchestration & Governance:** The entire pipeline's dependency graph is scheduled and monitored via **Lakeflow Jobs**, while **Unity Catalog** enforces data governance, fine-grained access control, and complete data lineage tracking across the platform.
