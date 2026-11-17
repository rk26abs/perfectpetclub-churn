# PerfectPetClub – Customer Churn ETL & Dashboard

Data Engineering Team Presentation, University of Hertfordshire.

> **Status:** repository set up. Team members are adding their sections branch by branch (see `GIT_WORKFLOW.md`).

The ETL pipeline turns four raw PerfectPetClub sources into a clean, **one-row-per-customer** Parquet dataset for analysing
**customer churn** (a customer cancelling the auto-renew of their subscription). A Matplotlib dashboard then visualises that dataset.

## Team

| Member | Student ID | Responsibility |
|---|---|---|
| Bhavana Padala | 25111063 | **Extract**: SQLite, CRM, server log and survey extract functions |
| Siddu Gundeboina | 25101571 | **Transform A**: data quality checks and cleaning/parsing |
| Pravalika | (add ID) | **Transform B + Load**: feature engineering, model-ready encoding, Parquet load |
| Kavya Sree Bingi | 26001523 | **Dashboard**: Matplotlib churn dashboard (`02_Churn_Dashboard.ipynb`) |
| Ramya Sree Kasula | 26000700 | **Repository**: Git setup, README, requirements, branching workflow, merging |
| Denya Sree Adapa | 25111595 | **Testing & release**: test section, merging branches, run-all and HTML export |

## Repository structure

```
├── 01_ETL_Pipeline.ipynb        # Extract → Data quality → Clean → Feature engineering → Load → Tests
├── 02_Churn_Dashboard.ipynb     # Matplotlib dashboard built from the Parquet output
├── PerfectPetClub_Churn_Final.ipynb  # FINAL: all parts combined, executed (submission)
├── data/                        # raw source files (read-only inputs)
│   ├── PerfectPetClub.db
│   ├── PerfectPetClub_CRM_report_1.csv
│   ├── PerfectPetClub_server_log_1.csv
│   ├── PerfectPetClub_survey.csv
│   └── PerfectPetClub_survey_questions.txt
├── output/                      # generated: customer_churn.parquet, model-ready parquet, figures/
├── html/                        # PerfectPetClub_Churn_Final.html = file submitted on Canvas
├── run_and_export.py            # runs both notebooks top-to-bottom and exports HTML
├── requirements.txt
└── GIT_WORKFLOW.md
```

## How to run

```bash
python -m venv .venv && source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter lab                                            # run 01 then 02, or:
python run_and_export.py                               # executes both notebooks and writes html/*.html
```

## Pipeline summary

| Stage | Functions | Output |
|---|---|---|
| Extract | `extract_sqlite`, `extract_csv`, `extract_survey_questions`, `extract_all` | dict of raw DataFrames |
| Data quality | `run_quality_checks` (41 checks: completeness, uniqueness, validity, consistency, referential integrity) | DQ report; stops on ERROR |
| Clean | `parse_platform`, `parse_user_agent`, `parse_income`, `clean_db`, `clean_crm`, `clean_server_log`, `clean_survey` | typed, tidy tables |
| Features | `build_subscription/sales/review/crm/engagement/survey_features`, `build_customer_dataset` | 221 × 48 customer table |
| Load | `load_parquet` (write + read-back verification), `encode_for_modelling` | `output/customer_churn.parquet`, `output/customer_churn_model_ready.parquet` |
| Tests | 17 unit, error-handling and output-contract tests | PASS/FAIL report |

**Key findings:** churn is 38.9% (86/221). Early (Jan–Feb) cohorts churn most. Promoters churn less than Passives.
Per-month service metrics are similar for churned and retained customers. The survey's Q3 (`satisfaction_score`) has the
same wording as Q2 (NPS), which is a data quality issue we flag.

See section 7 of `PerfectPetClub_Churn_Final.ipynb` for design decisions, limitations and future improvements.
