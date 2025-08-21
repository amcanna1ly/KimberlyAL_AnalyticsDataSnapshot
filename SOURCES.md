# Sources & Data Provenance

This project draws only on public datasets. Each chart includes a source/date footer and the exact files/filters are described here.

## Housing

- **Zillow Research — Zillow Home Value Index (ZHVI), ZIP-level**
  - Landing page: https://www.zillow.com/research/data/
  - Dataset: ZIP-level ZHVI (typical home value), monthly, smoothed/seasonally-adjusted
  - ZIPs used: **35091** (Kimberly), **35116** (Morris), **35180** (Warrior)
  - Notes: ZIP areas do not exactly match city boundaries; used here for local market trend context.
  - Retrieved: Aug 21, 2025

## Education

- **Alabama State Department of Education (ALSDE) — Supporting Data (2024)**
  - Landing page: https://reportcard.alsde.edu/SupportingData.aspx?ReportYear=2024&SystemCode=000&SchoolCode=0000
  - Tables used (CSV exports):
    - **Accountability** — provides *Graduation Rate*, *College & Career Readiness*, *Academic Growth*, *Chronic Absenteeism*.
    - **Educator Credentials / Experienced** — provides experienced vs. novice educator rates/counts.
    - *(Optional)* **CCR/Graduation** — not required when Accountability is present.
  - Filters used for school-level analysis:

    - **System:** *Jefferson County* (code **037**) — or export **All Systems/All Schools** and filter in code.

    - **School:** *Mortimer Jordan High School* (code **0610**).

    - **Group slices:** *All* Grade, *All* Gender, *All* Race, *All* Ethnicity, *All* Sub Population.

  - Guardrail: Exports with SystemCode=**000** / SchoolCode=**0000** are statewide-only snapshots and will not include school rows.
  - Retrieved: Aug 21, 2025

## Reproducibility

- Raw files live in `data/raw/` (not committed). Small, derived CSVs are written to `data/processed/` and may be committed.
- Notebooks and scripts include checks to alert if a statewide-only export is loaded.
- Each chart footer stamps the source and the latest month/year used.
