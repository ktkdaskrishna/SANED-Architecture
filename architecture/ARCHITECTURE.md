# PASI Platform — Architecture Document

> **Social Protection Fund Analytics & Decision Platform**
> Government of Oman — Data Lakehouse · MLOps · AI Agentic Platform

---

## 📋 Document Control

| Property | Value |
|---|---|
| **Platform** | PASI — Protection Analytics & Social Intelligence |
| **Organization** | Social Protection Fund, Sultanate of Oman |
| **Architecture Pattern** | Secure Data Lakehouse + MLOps + AI Agentic |
| **Scale** | ~4.5M persons, ~1.2M households |
| **Version** | 1.0 |
| **Date** | 2026-04-01 |

---

## 🏗️ Architecture Diagrams

Interactive HTML diagrams are available in `diagrams/`:

| # | Diagram | File | Description |
|---|---------|------|-------------|
| 1 | **High-Level Architecture** | [01-high-level-architecture.html](diagrams/01-high-level-architecture.html) | 8 logical layers from source integration to security |
| 2 | **End-to-End Data Flow** | [02-data-flow.html](diagrams/02-data-flow.html) | 7-stage pipeline from ministry ingestion to AI decisions |
| 3 | **Lakehouse Storage Zones** | [03-lakehouse-zones.html](diagrams/03-lakehouse-zones.html) | Bronze → Silver → Gold medallion architecture |
| 4 | **MLOps Pipeline** | [04-mlops-pipeline.html](diagrams/04-mlops-pipeline.html) | Feature engineering → Training → Registry → Deploy → Monitor |
| 5 | **AI Agentic Platform** | [05-ai-agentic-platform.html](diagrams/05-ai-agentic-platform.html) | 5-tier agent architecture with governed tools |
| 6 | **Security & Governance** | [06-security-governance.html](diagrams/06-security-governance.html) | 6 security domains + compliance mapping |

---

## 🎯 Architecture Overview

### Platform Pattern: Secure Data Lakehouse

The platform uses a **lakehouse architecture** (not warehouse-only) because:

- Multi-format raw ingestion from 8+ source domains
- Iterative ML pipelines require feature reuse and large-scale joins
- Adaptive social protection needs both operational and analytical perspectives
- Schema flexibility at ingestion → governed structure at serving

### 8 Logical Layers

```
┌─────────────────────────────────────────────────────────────────┐
│ L1  SOURCE INTEGRATION    Ministry APIs, file drops, events    │
├─────────────────────────────────────────────────────────────────┤
│ L2  DATA LAKE STORAGE     Raw → Standardized → Curated zones  │
├─────────────────────────────────────────────────────────────────┤
│ L3  PROCESSING & ETL      Spark, DQ, entity resolution        │
├─────────────────────────────────────────────────────────────────┤
│ L4  INTEGRATED REGISTRY   Golden records, historized entities  │
├─────────────────────────────────────────────────────────────────┤
│ L5  RULES & ML ENGINE     Eligibility, scoring, MLOps         │
├─────────────────────────────────────────────────────────────────┤
│ L6  ANALYTICS & SERVING   SQL, dashboards, APIs, reports      │
├─────────────────────────────────────────────────────────────────┤
│ L7  AI AGENTIC PLATFORM   Arabic/English chat, agent builder  │
├─────────────────────────────────────────────────────────────────┤
│ L8  SECURITY & GOVERNANCE IAM, encryption, audit (cross-cut)  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔌 Layer 1: Source Integration

### Source Domains (8)

| Domain | Integration Method | Frequency | Protocol |
|--------|-------------------|-----------|----------|
| Civil Identity & Family | REST API | Real-time / Daily | OAuth 2.0 |
| Education (Enrollment) | SFTP Batch | Daily | SFTP + PGP |
| Health (Birth/Death/Disability) | Event Stream | Real-time | HL7/FHIR, Kafka |
| Labor & Employment | DB Replication | Daily | CDC (Debezium) |
| Geography & Address | API | Weekly | REST |
| Finance & Payments | Batch File | Daily | SFTP + Schema Contract |
| Institutional Capacity | API/Batch | Weekly | REST / CSV |
| CSR Contributions | Manual Upload | On-demand | Secure Upload Portal |

### Integration Standards
- Every source captures: owner, schema, refresh cadence, sensitivity, legal sharing basis
- Schema contract validation before first ingestion
- Authentication required on all interfaces
- Monitoring, replay, and retry controls mandatory

---

## 📦 Layer 2: Data Lake Storage

### Medallion Architecture (Bronze → Silver → Gold)

| Zone | Purpose | Mutability | Format | Retention | Access |
|------|---------|-----------|--------|-----------|--------|
| **Bronze (Raw)** | Immutable source capture | Append-only | JSON, CSV, Parquet | 7+ years | Data Engineers |
| **Quarantine** | Failed validation records | Append-only | Same as source | Until resolved | Stewards |
| **Silver (Standardized)** | Cleansed, validated, resolved | Upsert | Delta Lake / Iceberg | 3 years | Data Engineers |
| **Gold (Curated)** | Analytics-ready, governed | Managed | Delta Lake / Iceberg | Active + 1 year | Analysts, APIs |

### Technology Stack
- **Object Storage**: S3 / Azure Data Lake Storage Gen2
- **Table Format**: Delta Lake or Apache Iceberg (ACID, time travel, schema evolution)
- **Metadata Catalog**: Unity Catalog / AWS Glue (lineage, classification, permissions)

---

## ⚡ Layer 3: Processing & ETL

### Core Engines

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Batch/Streaming ETL | Apache Spark | Distributed joins, aggregations, transformations |
| Data Quality | Great Expectations / Soda | Schema, freshness, plausibility, referential integrity |
| Entity Resolution | Splink / custom | Deterministic + probabilistic person matching |
| Household Builder | Graph engine | Family linkage, residence, dependency construction |
| CDC Pipeline | Debezium / Spark Streaming | Incremental processing from source changes |
| Schema Evolution | Delta Lake native | Automated schema migration and versioning |

### Processing Rules
- Records failing validation → quarantine zone (never silently accepted)
- SCD Type-2 historization for all registry entities
- Lineage metadata emitted at every transformation stage

---

## ⭐ Layer 4: Integrated Registry

### Core Entities (14)

| Entity | Description | History |
|--------|-------------|---------|
| **Person** | National ID, demographics, golden record | SCD-2 |
| **Household** | Composition from family, residence, dependency | SCD-2 |
| **Relationship** | Parent-child, spouse, dependent linkages | Event-based |
| **Address** | Physical location with geographic codes | SCD-2 |
| **Student Status** | Enrollment, institution, attendance | Temporal |
| **Employment Status** | Job, employer, wage indicators | Temporal |
| **Income Indicator** | Proxy scores, wage, benefit amounts | Temporal |
| **Benefit Program** | Definitions, rules, active windows | Versioned |
| **Eligibility Decision** | Status, reasons, evidence, review | Immutable log |
| **Payment Reference** | Disbursement, reconciliation | Immutable log |
| **Institution** | Schools, hospitals, CSR partners, capacity | SCD-2 |
| **Capacity Indicator** | Beds, enrollment, service capacity | Temporal |
| **Life Event** | Birth, death, marriage, enrollment change | Immutable |
| **Geographic Hierarchy** | Governorate → Wilayat → Area → Block | Slow-change |

---

## ⚖️ Layer 5: Rules & ML Engine

### 5A — Eligibility Rules Engine

- **No-code definitions**: Boolean logic, thresholds, field-based rules
- **Supported fields**: Age, education status, income proxy, household size, disability, region, employment, benefit history
- **Features**: Lookback windows, effective dates, overrides, simulation, version control
- **Workflow**: Draft → Simulate → Approve → Production
- **Execution**: Batch (scheduled) and on-demand

### 5B — MLOps Pipeline (6 Phases)

```
Feature Engineering → Model Training → Model Registry → Governance Gate → Deployment → Monitoring
```

| Phase | Key Activities |
|-------|---------------|
| **Feature Engineering** | Extract from Gold zone, temporal windows, household aggregations, Feature Store publish |
| **Model Training** | Experiment tracking (MLflow), hyperparameter tuning, cross-validation, fairness evaluation, SHAP |
| **Model Registry** | Version management, stage transitions, artifact storage, model card generation |
| **Governance Gate** | Review board approval, bias audit, explainability check, security scan, compliance validation |
| **Deployment** | Canary rollout (10%→100%), A/B testing, shadow mode, batch scoring, real-time endpoints |
| **Monitoring** | Data drift detection, concept drift alerts, performance degradation, auto-retrain triggers |

### ML Model Inventory

| Model | Type | Output | Use Case |
|-------|------|--------|----------|
| Household Similarity | Clustering | Cluster labels | Population segmentation |
| Need Prioritization | Ranking | Priority score (0-100) | CSR allocation |
| Benefit Leakage | Anomaly Detection | Anomaly flags | Fraud / duplication |
| Vulnerability Proxy | Regression | Vulnerability index | Poverty estimation |
| Early Warning | Classification | Risk category | Unmet needs alerting |

---

## 📊 Layer 6: Analytics & Serving

### Channels

| Channel | Technology | Audience |
|---------|-----------|----------|
| SQL Warehouse | Databricks SQL / Synapse SQL | Analysts, notebooks |
| BI Dashboards | PowerBI / Apache Superset | Executives, operators |
| REST/GraphQL APIs | API Gateway + microservices | Downstream systems |
| Scheduled Reports | PDF, Excel, email delivery | Program administrators |
| Event Publisher | Kafka / Event Hub | Real-time notifications |

### Key Dashboards
- Enrollment vs. served population by program
- Regional distribution and coverage gaps
- Budget utilization and forecasting
- Program overlap and duplication risk
- CSR allocation outcomes
- Segment trends by demographics

---

## 🤖 Layer 7: AI Agentic Platform

### 5-Tier Architecture

| Tier | Components |
|------|-----------|
| **Presentation** | Arabic/English Web Chat, Mobile PWA, API Gateway, Notification Hub |
| **Orchestration** | LangGraph Engine, Router Agent, Conversation Memory, Agent Builder |
| **Tools & Knowledge** | Eligibility Lookup, Segment Query, CSR Allocator, Policy RAG, Institution Query, Analytics Query |
| **Safety & Governance** | Input Filter, PII Shield, Permission Gate, Human-in-the-Loop, Audit Logger |
| **Foundation** | LLM Gateway (GPT-4o/Claude/Gemini), Vector Store, Inference Cache, Usage Metrics |

### Pre-configured Agents

| Agent | Scope | Approval |
|-------|-------|----------|
| Eligibility Agent | Person/household eligibility lookup | Auto |
| Segment Explorer | Population segment query and comparison | Auto |
| CSR Allocator | Budget-constrained fund allocation | Human Review |
| Coverage Analyst | Gap analysis and regional distribution | Auto |
| Hospital Support | Institutional capacity and need analysis | Human Review |
| Policy Advisor | Regulation and policy Q&A | Auto |

### Agent Execution Flow (CSR Example)

```
User Query (Arabic) → Auth Check → Intent Route → Tool Calls 
  → Scoring/Ranking → Explanation → Human Approval → Action
```

---

## 🛡️ Layer 8: Security & Governance

### 6 Security Domains

| Domain | Key Controls |
|--------|-------------|
| **Identity & Access** | SSO, MFA, RBAC+ABAC, service identities, JIT admin, quarterly access reviews |
| **Encryption & Keys** | TLS 1.3 transit, AES-256 rest, CMK, domain separation, automated rotation |
| **Data Protection** | National ID tokenization, column masking, row-level security, 4-level classification |
| **AI Safety** | No raw PII access for models, governed tools only, prompt audit, human approval gates |
| **Audit & Observability** | Immutable logs, Prometheus/Grafana, SIEM integration, access anomaly detection |
| **Compliance & Governance** | Data residency (Oman), retention policies, segregation of duties, model governance |

---

## 📐 Technology Stack Summary

| Layer | Primary Technologies |
|-------|---------------------|
| Integration | Apache Kafka, Debezium, REST APIs, SFTP |
| Storage | S3/ADLS Gen2, Delta Lake/Iceberg, Parquet |
| Catalog | Unity Catalog / AWS Glue |
| Processing | Apache Spark, Great Expectations, Splink |
| ML | MLflow, Spark ML, scikit-learn, XGBoost |
| Serving | Databricks SQL / Synapse, PowerBI / Superset |
| AI | LangGraph, GPT-4o / Claude / Gemini, FAISS/Pinecone |
| Security | Entra ID / Keycloak, HashiCorp Vault, Prometheus, Grafana |
| Infra | Kubernetes (AKS/EKS), Terraform, GitHub Actions |

---

## 🗓️ Release Roadmap

| Phase | Scope | Timeline |
|-------|-------|----------|
| **Phase 1: Foundation** | Cloud landing zone, core lakehouse, initial sources, person/household registry, basic dashboards, IAM/encryption/audit | Months 1-6 |
| **Phase 2: Policy Automation** | Rules engine, segment builder, admin console, student/newborn workflows, explainability | Months 4-9 |
| **Phase 3: Advanced Intelligence** | ML pipelines, recommendations, CSR allocation, Arabic/English AI assistant, agent builder | Months 7-12 |
| **Phase 4: Ecosystem Maturity** | Broader ministry integrations, institutional use cases, advanced forecasting, downstream automation | Months 10-15 |

---

## 📊 Data Volume Estimates

| Entity | Current Records | Growth/Year |
|--------|----------------|------------|
| Persons | ~4.5M | +2% |
| Households | ~1.2M | +2% |
| Life Events | ~50M | +15% |
| Eligibility Decisions | ~20M | +25% |
| Student Records | ~800K | +3% |
| Institutions | ~5K | +5% |
| Audit Events | ~100M/yr | +30% |

---

## ❓ Open Architecture Questions

1. Which datasets are legally approved for household linkage and vulnerability scoring?
2. Which use cases require near-real-time processing vs. daily/monthly batch?
3. What are the authoritative source systems for identity, household, and address?
4. Which fields can be exposed in CSR scenarios vs. internal-only ranking?
5. What approval thresholds exist before AI recommendations produce operational outputs?
6. Cloud provider selection: Azure (Synapse/Databricks) vs. AWS (EMR/Glue) vs. hybrid?
7. LLM hosting: sovereign deployment vs. managed API with data residency guarantees?
