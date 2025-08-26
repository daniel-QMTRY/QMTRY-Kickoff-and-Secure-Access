Absolutely—here’s the **complete, fixed README.md** ready to paste into your repo. All Mermaid blocks are GitHub-safe (labels quoted, no `&`, clean punctuation).

````markdown
# QMTRY — Kickoff and Secure Access

> **30-minute discovery, scope, and BAA.** We agree on a secure transfer (**SFTP / S3 / Azure Blob / BigQuery**) and use **Synthea** synthetic data for demos — **no PHI in examples**.

[🌐 See our onboarding](https://qmtry.ai/compliance/) • Contact: contracts@qmtry.com

---

## Why this repo exists

Healthcare leaders want a **safe, fast path** to evaluate QMTRY. This repo documents that path: how we meet, sign the BAA, stand up **secure data ingress**, validate with **synthetic Synthea** data, and show an **audit-ready** demo without touching PHI.

---

## Table of Contents

- [Executive Summary](#executive-summary)  
- [Onboarding Flow (30-min → demo)](#onboarding-flow-30-min--demo)  
- [Security and Compliance Controls](#security-and-compliance-controls)  
- [Architecture and Data Flow](#architecture-and-data-flow)  
- [Timeline (Gantt)](#timeline-gantt)  
- [Effort Breakdown (Pie)](#effort-breakdown-pie)  
- [What to Bring to the Call](#what-to-bring-to-the-call)  
- [Secure Transfer Recipes](#secure-transfer-recipes)  
- [Evidence Bundle (What you receive)](#evidence-bundle-what-you-receive)  
- [Appendix: Sample Data Contract](#appendix-sample-data-contract)  
- [Repo Structure (suggested)](#repo-structure-suggested)  
- [FAQ](#faq)  
- [License](#license)

---

## Executive Summary

- **Goal:** in one short session, align on scope and security, execute a **BAA**, and pick your **secure ingress** option.  
- **Demo Safety:** we use **Synthea** data end-to-end for examples; **no PHI** enters our systems during evaluation.  
- **Outcome:** a reproducible, evidence-backed demo and a ready path to production if you choose to proceed.

---

## Onboarding Flow (30-min → demo)

```mermaid
flowchart TD
    A["Discovery Call (30 min)"] --> B["Scope and Success Criteria"]
    B --> C["BAA Execution"]
    C --> D{"Choose Secure Ingress"}
    D --> D1["SFTP"]
    D --> D2["S3 Bucket (AWS)"]
    D --> D3["Azure Blob"]
    D --> D4["BigQuery External Table"]
    D1 --> E["Ingress Policy + Keys/Role"]
    D2 --> E
    D3 --> E
    D4 --> E
    E --> F["Synthea Demo Dataset (No PHI)"]
    F --> G["Validation Checks (Schema, Row Counts, Nulls)"]
    G --> H["Demo Notebook and Dashboard"]
    H --> I["Evidence Bundle (audit-ready)"]
````

---

## Security and Compliance Controls

* **BAA** before any non-public data.
* **No PHI for demos** — Synthea synthetic data only.
* **Encryption in transit and at rest** (TLS, server-side encryption / SSE-S3 / SSE-KMS / Azure SSE).
* **Least-privilege access** (scoped IAM roles, time-boxed credentials).
* **Data minimization and retention**: demo artifacts kept only as long as necessary.
* **Audit trail**: logs for ingress, validation, and access.

---

## Architecture and Data Flow

```mermaid
sequenceDiagram
    participant C as Customer
    participant I as "IAM / Secrets"
    participant IN as "Secure Ingress\n(SFTP | S3 | Azure | BigQuery)"
    participant S as Staging (Read-only)
    participant V as Validation
    participant D as "Demo Apps"
    C->>I: Exchange keys / roles (least-privilege)
    C->>IN: Provide dataset location (Synthea only)
    IN->>S: Encrypted transfer (TLS + server-side encryption)
    S->>V: Schema and quality checks
    V->>D: Publish demo-safe tables
    D->>C: Executive walkthrough + Q and A
```

---

## Timeline (Gantt)

```mermaid
gantt
    title Kickoff to Demo
    dateFormat  YYYY-MM-DD
    section Meeting
    Discovery_30_min           :milestone, m1, 2025-08-26, 1d
    section Legal
    BAA_Execution              :active, l1, 2025-08-26, 1d
    section Security
    Ingress_Setup              :s1, after l1, 1d
    Validation_Synthea         :s2, after s1, 1d
    section Demo
    Executive_Demo             :milestone, d1, after s2, 1d
```

---

## Effort Breakdown (Pie)

```mermaid
pie title Effort Allocation (Evaluation Phase)
    "Discovery and Scope" : 15
    "Security and Ingress" : 35
    "Validation" : 25
    "Demo Prep and Walkthrough" : 25
```

---

## What to Bring to the Call

* Your **goal** (e.g., Stars, HEDIS, UM/RCM, population health).
* **Preferred cloud** (AWS/Azure/GCP) and **ingress option**.
* A **technical contact** for IAM/keys.
* Any **reporting deadlines** and success criteria.

---

## Secure Transfer Recipes

> Pick the path that matches your environment. All examples below assume **synthetic Synthea** data.

### 1) SFTP (simple and reliable)

```bash
# Upload (client-side)
sftp -i /path/to/key demo_ingress@ingress.qmtry.ai <<'EOF'
put ./synthea/*.csv /incoming/synthea/
bye
EOF
```

### 2) AWS S3 (customer-owned bucket, read-only role for QMTRY)

```bash
# Example: create a bucket and upload (customer side)
aws s3 mb s3://qmtry-demo-synthea --region us-east-1
aws s3 sync ./synthea s3://qmtry-demo-synthea/synthea/
```

> We exchange exact IAM ARNs and a least-privilege bucket policy as part of the BAA/ingress worksheet.

### 3) Azure Blob Storage

```bash
# Upload with az CLI
az storage blob upload-batch \
  --account-name <yourAccount> \
  --destination 'qmtry-demo/synthea' \
  --source ./synthea
```

### 4) BigQuery External Table (point to data without copying)

```sql
-- Customer creates an external table from GCS with synthetic CSVs
CREATE EXTERNAL TABLE `demo.synthea_patients`
WITH CONNECTION `us.gcs.my_conn`
OPTIONS (
  format = 'CSV',
  uris = ['gs://qmtry-demo/synthea/patients/*.csv'],
  skip_leading_rows = 1
);
```

---

## Evidence Bundle (What you receive)

* **Ingress checklist** (who/what/when, key custody, TTL)
* **Validation report** (schema, row counts, null %, type checks)
* **Run log** (timestamped)
* **Demo workbook** (queries + visuals)
* **Security summary** (controls applied, retention)

All items are **exportable** for auditors.

---

## Appendix: Sample Data Contract

| Field            | Type   | Required | Notes                            |
| ---------------- | ------ | -------: | -------------------------------- |
| `patient_id`     | STRING |        ✅ | Surrogate ID (synthetic)         |
| `dob`            | DATE   |        ✅ | No real DOB in demos (Synthea)   |
| `gender`         | STRING |        ✅ | Coded per Synthea                |
| `encounter_date` | DATE   |        ✅ |                                  |
| `code_system`    | STRING |        ✅ | e.g., ICD-10, RxNorm (synthetic) |
| `code`           | STRING |        ✅ |                                  |
| `value`          | STRING |        ✅ | Observation or metric            |

> **PHI Policy (Demo):** Synthea only. No direct or indirect identifiers.

---

## Repo Structure (suggested)

```
.
├─ README.md
├─ evidence-template/
│  ├─ ingress-checklist.md
│  ├─ validation-report.md
│  └─ run-log.sample.json
├─ ingress-examples/
│  ├─ sftp/
│  ├─ aws-s3/
│  ├─ azure-blob/
│  └─ bigquery/
└─ demo-notebooks/
   └─ synthea-demo.md
```

---

## FAQ

* **Can we use real PHI in the pilot?** Not for the initial demo. We begin with Synthea. PHI only after BAA + security sign-off.
* **How long until a demo?** Typically 2–4 business days after BAA and ingress setup.
* **What do we keep?** Only the minimum. Demo data can be deleted immediately on request.

---

## License

MIT (for this documentation). Your data and any access credentials remain yours.

```


