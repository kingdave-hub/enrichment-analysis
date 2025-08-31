
# Healthcare Database Mini-Project (PostgreSQL)

**Purpose:** Portfolio-ready project to demonstrate **database design**, **ETL**, **SQL analytics**, and **healthcare metrics** (readmissions, ED→IP conversion, medication utilization).

## Contents
- `schema.sql` — normalized schema with constraints, indexes, a readmission **view**, and a toy **sepsis flag** function.
- `load_data.sql` — `COPY` commands to load synthetic CSVs.
- `example_queries.sql` — curated analytics queries you can demo.
- `patients.csv`, `staff.csv`, `visits.csv`, `diagnoses.csv`, `medications.csv`, `meds_admin.csv`, `labs.csv` — synthetic data (~60 patients, 230 visits).
- `analytics_example.py` — a tiny Python script that connects to Postgres and plots visits per month + prints 30-day readmission rate.

## Quickstart (PostgreSQL)
```bash
# 1) Start Postgres (local or Docker). Example using Docker:
docker run --name pg -e POSTGRES_PASSWORD=postgres -p 5432:5432 -d postgres:16

# 2) Copy files into the container (or mount a volume), then in psql:
psql -h localhost -U postgres -d postgres -f schema.sql

# 3) Load seed data
psql -h localhost -U postgres -d postgres -f load_data.sql

# 4) Explore analytics
psql -h localhost -U postgres -d postgres -f example_queries.sql
```

## Talking Points for Interviews
- **Design choices:** `patients` ↔ `visits` (1:M), `visits` ↔ `diagnoses` (1:M), medication admin as a **bridge**.
- **Data quality:** check constraints (date order, sex categories), unique keys (MRN/NPI).
- **Performance:** indexes on FKs; prebuilt **view** for readmissions; could add **materialized views**.
- **Security:** mention **role-based access** (read-only analytics role), PHI handling & de-identification for public sharing.
- **Extensibility:** dimension tables (`dim_icd10`, `dim_rxnorm`) for joins to descriptions, mapping to KPIs.

## Stretch Ideas (to make it "top notch")
- Add **dbt** models for transformations and tests.
- Build a **Power BI** or **Tableau** dashboard off the Postgres data (readmissions over time, top diagnoses).
- Write unit tests in **pytest** for SQL outputs (row counts, not-null, business rules).
- Add **row-level security** policy examples for HIPAA-style minimum access.
- Implement **CDC NHSN-like** logic for device-associated infections (toy version).
- Package synthetic data generator (this repo) so reviewers can **reproduce** results.

---

© 2025 Your Name. Educational use only.
