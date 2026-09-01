# Collections Recovery Analysis

> End-to-end analysis of collections recovery performance, data quality,
> attribution, and executive reporting.

------------------------------------------------------------------------

## Executive Overview

This project evaluates collections recovery performance through a
reproducible analytical workflow using **SQL, DuckDB, Python, and
Tableau**.

The analysis transforms operational data into a curated analytical
layer, validates key recovery metrics, investigates data-quality and
attribution risks, evaluates performance trends, and translates the
findings into executive reporting.

### Business Questions

The analysis is designed to answer:

1.  **What happened?**
2.  **Why did it happen?**
3.  **How confident are we in the reported performance?**
4.  **What should management do next?**

------------------------------------------------------------------------

## Executive Snapshot

  KPI                                  Result
  -------------------------- ----------------
  **Total Recovered Cash**     **₹131.65 Cr**
  **Recovery Rate**                 **70.0%**
  **Successful Payments**          **17,545**
  **Reported Improvement**         **11.11%**

### Headline Finding

Recovered cash remained broadly stable through January--July before a
sharp decline in August.

The analysis evaluates whether the reported **11.11% improvement**
represents a sustained improvement in underlying recovery performance or
a single-period movement influenced by data quality, attribution, or
denominator effects.

> **Note:** KPI values and final conclusions should be interpreted
> together with the validation, data-quality analysis, and Executive
> Memo.

------------------------------------------------------------------------

# Analytical Approach

The project follows a layered workflow from source data to executive
decision-making:

``` text
Source Data
     │
     ▼
DuckDB
     │
     ▼
SQL Staging & Cleaning
     │
     ▼
Golden Analytical Layer
     │
     ├──────────────► Data Quality Investigation
     │
     ▼
Metric Calculation
     │
     ├──────────────► Python Analysis
     │
     └──────────────► Tableau Dashboard
                              │
                              ▼
                       Executive Reporting
```

The workflow separates production transformation logic from forensic
data-quality investigations so that analytical results remain traceable
and auditable.

------------------------------------------------------------------------

# Repository Structure

``` text
finale_project/
│
├── README.md
├── .gitignore
│
├── 01_sql_repository/
│   ├── README.md
│   ├── 01_staging/
│   ├── 02_golden/
│   ├── 03_metrics/
│   └── 04_analysis/
│
├── 02_analysis_notebook/
│   ├── analysis_notebook.ipynb
│   ├── chart_hourly_answer_rate.png
│   ├── chart_mom_steps.png
│   ├── chart_monthly_recovery.png
│   └── collections.duckdb
│
├── 03_golden_dataset/
│   ├── dataset/
│   └── golden_dataset_description.*
│
├── 04_data_quality/
│   └── data_quality_report.md
│
├── 05_executive_dashboard/
│   ├── tableau/
│   └── executive_dashboard.png
│
├── 06_executive_memo/
│   └── executive_memo.docx
│
├── 07_architecture/
│   ├── production_design.*
│   └── architecture_diagram.*
│
└── pipeline/
    ├── profile_raw.py
    ├── profile_summary.csv
    └── run_pipeline.py
```

> The exact filenames of generated artifacts may vary; the structure
> above reflects the intended organization of the submission.

------------------------------------------------------------------------

# 01 --- SQL Repository

The SQL repository contains the reproducible data preparation and
analytical logic used throughout the project.

### SQL layers

  Layer           Purpose
  --------------- --------------------------------------
  `01_staging`    Initial cleaning and standardization
  `02_golden`     Curated analytical entities
  `03_metrics`    Business metric calculations
  `04_analysis`   Analytical and investigative queries

The SQL layer covers:

-   Data cleaning
-   Standardization
-   Deduplication
-   Golden-layer construction
-   Metric calculations
-   Analytical queries
-   Data-quality investigations

The SQL repository also contains its own README documenting the SQL
workflow.

------------------------------------------------------------------------

# 02 --- Analysis Notebook

The analysis notebook documents the analytical reasoning behind the
final findings.

It is intended to show the analytical process rather than only the final
charts.

### Analysis includes

-   Exploratory analysis
-   KPI validation
-   Monthly recovery trends
-   Month-on-month performance analysis
-   Hour-of-day answer-rate analysis
-   Recovery performance analysis
-   Attribution investigation
-   Interpretation of findings

### Key Analytical Visuals

#### Month-on-Month Recovery Movement

![Month-on-Month Recovery
Steps](02_analysis_notebook/chart_mom_steps.png)

#### Answer Rate by Hour

![Answer Rate by
Hour](02_analysis_notebook/chart_hourly_answer_rate.png)

#### Monthly Recovered Cash

![Monthly Recovered
Cash](02_analysis_notebook/chart_monthly_recovery.png)

------------------------------------------------------------------------

# 03 --- Golden Dataset

The Golden Dataset represents the curated analytical layer generated
from the source data.

It provides standardized and validated entities and fields for
downstream analysis and reporting.

The golden layer serves as the analytical foundation for:

-   Recovery metrics
-   Payment analysis
-   Campaign attribution
-   Operational analysis
-   Executive reporting

The accompanying dataset documentation describes the resulting
analytical layer and its intended use.

------------------------------------------------------------------------

# 04 --- Data Quality Report

Data quality is treated as a core analytical component of the project.

The investigation evaluates whether issues in the underlying operational
data could materially affect recovery reporting or management
conclusions.

### Areas Investigated

-   Duplicate payments
-   Campaign attribution ambiguity
-   Timezone inconsistencies
-   Vendor/disposition mapping
-   Agent identity quality
-   Portfolio mix
-   Targeting and conversion denominators

The detailed report is available at:

`04_data_quality/data_quality_report.md`

Supporting forensic SQL checks are maintained within:

`01_sql_repository/`

### Payment Data Quality

The payment investigation distinguishes between obvious duplicate
ingestion records and potentially legitimate records that share an
identifier.

The analysis identified:

-   **25,500 raw payment rows**
-   **25,000 distinct payment IDs**
-   **486 exact duplicate pairs**
-   **14 payment-ID collision pairs**

Exact duplicate pairs can be treated as duplicate/retry ingestion
records, while collision records are retained rather than automatically
deleted.

This approach prioritizes preserving potentially legitimate financial
events while preventing obvious duplicate records from inflating
recovery metrics.

------------------------------------------------------------------------

# 05 --- Executive Dashboard

The Tableau dashboard is designed as a **single-screen executive view**.

The objective is decision clarity rather than visual complexity.

### Dashboard Focus

-   Total recovered cash
-   Recovery rate
-   Successful payments
-   Reported improvement
-   Monthly recovery trend
-   Key business insight

![Executive Recovery
Dashboard](05_executive_dashboard/executive_dashboard.png)

The Tableau workbook is available in:

`05_executive_dashboard/tableau/`

------------------------------------------------------------------------

# 06 --- Executive Memo

The Executive Memo translates the analytical findings into
management-level conclusions and actions.

It addresses the required questions:

### What happened?

Summary of recovery performance and the major movements observed during
the analysis period.

### Why did it happen?

Assessment of operational, attribution, denominator, and data-quality
factors.

### How confident are we?

Separation of validated findings from areas affected by data-quality or
attribution uncertainty.

### What should management do?

Evidence-based recommendations focused on the highest-value
opportunities.

### What is the expected financial impact?

Quantification of the potential financial opportunity where supported by
the analysis.

The memo is intentionally concise and designed for executive
consumption.

------------------------------------------------------------------------

# 07 --- Architecture

The architecture documents the flow from operational data through
analytical processing and executive reporting.

### Analytical Architecture

``` text
                 SOURCE DATA
                      │
                      ▼
                  DuckDB
                      │
                      ▼
             SQL STAGING LAYER
                      │
                      ▼
              GOLDEN DATA LAYER
                      │
          ┌───────────┴───────────┐
          │                       │
          ▼                       ▼
   DATA QUALITY              METRICS
   INVESTIGATION                 │
          │                      │
          └───────────┬──────────┘
                      ▼
               ANALYTICAL LAYER
                      │
              ┌───────┴───────┐
              │               │
              ▼               ▼
           Python          Tableau
          Analysis        Dashboard
              │               │
              └───────┬───────┘
                      ▼
             EXECUTIVE REPORTING
                      │
              ┌───────┴───────┐
              ▼               ▼
        Executive Memo   Management View
```

Detailed architecture and production-design documentation are available
in:

`07_architecture/`

------------------------------------------------------------------------

# Reproducible Pipeline

The `pipeline/` directory contains supporting Python scripts used for
data profiling and pipeline execution.

### Pipeline Components

  File                    Purpose
  ----------------------- --------------------
  `profile_raw.py`        Raw-data profiling
  `profile_summary.csv`   Profiling output
  `run_pipeline.py`       Pipeline execution

These scripts complement the SQL workflow and support reproducibility of
the analytical process.

------------------------------------------------------------------------

# Reproducibility

The project is structured so that the analytical workflow can be
reproduced from the supplied source data and the repository's
transformation logic.

### Workflow

``` text
Source Data
    ↓
DuckDB
    ↓
Raw Data Profiling
    ↓
SQL Staging
    ↓
Golden Layer
    ↓
Metric Calculation
    ↓
Data Quality Validation
    ↓
Python Analysis
    ↓
Tableau Dashboard
    ↓
Executive Reporting
```

### Reproduction Sequence

1.  Load the supplied source data into DuckDB.
2.  Profile the raw data using the pipeline utilities.
3.  Execute the SQL staging and cleaning logic.
4.  Build the Golden Analytical Layer.
5.  Calculate the required business metrics.
6.  Execute data-quality and validation checks.
7.  Run the analysis notebook.
8.  Validate the analytical outputs.
9.  Use the validated outputs for the Tableau dashboard.
10. Translate the findings into the Executive Memo.

The separation between transformation SQL, analytical queries,
data-quality checks, Python analysis, and reporting makes the workflow
easier to audit and reproduce.

------------------------------------------------------------------------

# Data & Confidentiality

The underlying source database was provided as part of the hiring
assessment.

The local DuckDB database is **excluded from version control** through
`.gitignore` and is not intended to be published in the repository.

The repository therefore focuses on the analytical methodology,
transformation logic, derived outputs, documentation, and reporting
artifacts required to demonstrate the work without redistributing the
original source database.

------------------------------------------------------------------------

# Tools & Technologies

  -----------------------------------------------------------------------
  Technology                          Role
  ----------------------------------- -----------------------------------
  **SQL**                             Data preparation, transformation,
                                      validation, and analysis

  **DuckDB**                          Local analytical database and SQL
                                      execution

  **Python**                          Profiling, exploratory analysis,
                                      validation, and visualization

  **Pandas**                          Data manipulation and analysis

  **Matplotlib**                      Analytical visualization

  **Tableau**                         Executive dashboarding

  **Git / GitHub**                    Version control and project
                                      documentation
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# Analytical Design Principles

### Reproducibility

Analytical results should be traceable from executive outputs back to
the transformation and validation logic.

### Data Quality Before Decision-Making

Reported KPIs are evaluated alongside data-quality, attribution, and
denominator risks.

### Preservation of Financial Events

Potentially legitimate financial records are not deleted simply because
identifiers collide.

### Separation of Layers

Cleaning, golden-layer construction, metric calculation, analysis,
validation, and reporting are logically separated.

### Decision-Oriented Reporting

The executive dashboard prioritizes the information required to
understand the situation and make decisions quickly.

### Evidence-Based Recommendations

Management recommendations are derived from analytical evidence rather
than from headline metrics alone.

------------------------------------------------------------------------

# Deliverables

  \#       Deliverable            Repository Location
  -------- ---------------------- ---------------------------
  **01**   SQL Repository         `01_sql_repository/`
  **02**   Analysis Notebook      `02_analysis_notebook/`
  **03**   Golden Dataset         `03_golden_dataset/`
  **04**   Data Quality Report    `04_data_quality/`
  **05**   Executive Dashboard    `05_executive_dashboard/`
  **06**   Executive Memo         `06_executive_memo/`
  **07**   Architecture Diagram   `07_architecture/`

------------------------------------------------------------------------

## Final Note

This repository demonstrates an end-to-end analytical workflow:

**Operational Data → Data Quality → Transformation → Golden Layer →
Analysis → Insight → Executive Decision**

The emphasis is on **traceability, analytical rigor, reproducibility,
and decision usefulness**.
