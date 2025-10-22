# Kimberly, AL — Public Data Snapshot

Neutral, replicable charts from public datasets so neighbors can see Kimberly’s trajectory.

> **Focus:** Housing trends, school outcomes, and local demographics/economics — sourced and reproducible.

---

## What’s inside

- **Housing (Zillow ZHVI, ZIP level)**  
  35091 with 35116/35180 for context (2000–2025).

- **Education (ALSDE 2024)**  
  - **MJHS vs OTM Comparison:** Mortimer Jordan HS vs Vestavia Hills HS vs Homewood HS  
    KPIs: Graduation, College & Career Readiness (CCR), Academic Growth Index, Chronic Absenteeism, Educator Experience (Experienced/Novice), CCR Cohort Size, and Avg Proficiency (ELA/Math/Science).
  - **MJHS single-school snapshots** (from earlier notebook).

- **Demographics & Economics (ACS 5-yr 2023 via API)**  
  ZCTAs **35091 / 35116 / 35180**: population, age mix, median household income, labor force %, unemployment %, owner-occupied %, median home value, median rent.

**Outputs:** small CSVs + high-resolution PNG charts (social-ready).

_Last updated: **Aug 23, 2025**_

---

## Quickstart

### 1) Environment

**macOS/Linux**
```bash
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
```

**Windows PowerShell**
```powershell
py -m venv .venv
. .venv/Scripts/Activate.ps1
pip install -r requirements.txt
```

**(Optional) Census API key** — avoids rate limits:
```bash
# macOS/Linux
export CENSUS_KEY=YOUR_KEY_HERE

# Windows PowerShell
$env:CENSUS_KEY="YOUR_KEY_HERE"
```

### 2) Generate ACS ZIP snapshot (CSV + charts)
```bash
python acs_zip_snapshot.py   --zips 35091 35116 35180   --labels "Kimberly (35091),Morris (35116),Warrior (35180)"   --outdir charts
```

### 3) Open notebooks (optional)
```bash
pip install jupyter
jupyter notebook
```
Then open:
- `notebooks/Kimberly_HomeValuations.ipynb`
- `notebooks/Kimberly_EduStats.ipynb`
- `notebooks/ALSDE_SchoolComparison-OTMvsNJ.ipynb`  ← **new**

---

## Repository layout

```
charts/
    ├── acs_2023_profile_ZCTA_35091_35116_35180.csv
    ├── acs_demographics_compare_35091_35116_35180.csv
    ├── acs_home_value.png
    ├── acs_income.png
    ├── acs_pop.png
    ├── acs_unemployment.png
    ├── alsde_compare_MJHS_Vestavia_Homewood.csv
    ├── HomeValueTrend.png
    ├── k12_ccr_cohort.png
    ├── k12_ccr_rate.png
    ├── k12_chronic_abs.png
    ├── k12_econ_disadv.png
    ├── k12_enrollment.png
    ├── k12_experienced.png
    ├── k12_grad_rate.png
    ├── k12_growth_overall.png
    ├── k12_growth.png
    ├── k12_novice.png
    ├── k12_prof_avg.png
    ├── k12_scorecard_MJHS_Vestavia_Homewood.png
    ├── MJHS_EduCredsBreakdown.png
    └── MJHS_Graduated_CCR.png
data/
    ├── processed/
        ├── Demographics/
            ├── acs_2023_profile_ZCTA_35091_35116_35180.csv
            └── acs_demographics_compare_35091_35116_35180.csv
        ├── Education/
            ├── alsde_compare_MJHS_Vestavia_Homewood.csv
            ├── mjhs_acc_kpi.csv
            └── mjhs_educator_credentials.csv
        └── Zillow_Home/
            └── zhvi_latest_snapshot.csv
    └── raw/
        ├── Education.7z
        └── Zillow_Home.7z
notebooks/
    ├── ALSDE_SchoolComparison-OTMvsNJ.ipynb
    ├── Kimberly_ACS_Snapshot.ipynb
    ├── Kimberly_EduStats.ipynb
    └── Kimberly_HomeValuations.ipynb
CITATION.cff
gitignore
README.md
requirements.txt
SOURCES.md
```

---

## ACS ZIP snapshot (what it does)

Fetches ACS 5-year **Data Profile** metrics for ZCTAs and writes:

- **CSV:** `acs_2023_profile_ZCTA_35091_35116_35180.csv`
- **Charts:**
  - `acs_pop.png` — Total population  
  - `acs_income.png` — Median household income  
  - `acs_home_value.png` — Median home value (owner-occupied)  
  - `acs_unemployment.png` — Unemployment rate

Chart labels use friendly names (e.g., “Kimberly (35091)”).  
**Note:** ZCTAs are ZIP-based Census areas; boundaries may differ from city limits.

_Optional:_
```bash
python acs_zip_snapshot.py --include-decennial-growth
# also fetches 2010→2020 Decennial population change & charts it
```

---

## Education notebooks

### ALSDE_SchoolComparison-OTMvsNJ.ipynb  ← **new**
**Compares:** Mortimer Jordan HS (Jefferson County) vs Vestavia Hills HS vs Homewood HS (ALSDE 2024, “All Students”).  
**Inputs (CSV):**  
- Accountability  
- CCR/Grad Rate  
- Educator Experienced (Credentials)  
- Proficiency *(optional; averaged across ELA/Math/Science if present)*

**Outputs to `charts/`:**
- `k12_grad_rate.png` — Graduation Rate  
- `k12_ccr_rate.png` — College & Career Readiness  
- `k12_prof_avg.png` — Avg Proficiency (ELA/Math/Science)  
- `k12_chronic_abs.png` — Chronic Absenteeism *(lower is better)*  
- `k12_growth.png` — Academic Growth (Index)  
- `k12_experienced.png` — Experienced Educators  
- `k12_novice.png` — Novice/Inexperienced *(lower is better)*  
- `k12_ccr_cohort.png` — CCR Cohort Size (student count)  
- `k12_scorecard_MJHS_Vestavia_Homewood.png` — **one-page multi-panel scorecard**

**Notes:**  
- Notebook prefers 4-year graduation values for “All Students” when available; otherwise falls back to best available.  
- Column names in ALSDE files sometimes vary; helper functions match common variants.  
- If statewide files (000/0000) are used, ensure rows include your **district/school**.

### Kimberly_EduStats.ipynb
- MJHS single-school views (Graduation/CCR, Experienced vs Novice staff).  
- Filters to **System = Jefferson County** and **School = Mortimer Jordan High School**, “All Students” where applicable.

### Kimberly_HomeValuations.ipynb
- Loads Zillow ZHVI ZIP-level data; builds a 2000–2025 line chart for **35091/35116/35180**.  
- Figure notes clarify “ZIP-level (not city boundary).”

---

## Reproducibility

`requirements.txt`
```
pandas
requests
matplotlib
jupyter
```

- All charts are generated from code; CSVs are committed for verification.  
- Each chart includes clear titles and a source footer; data vintage is noted here and in commit messages.  
- Scripts/notebooks are designed to run on a clean clone after placing raw data (if using flat files) into `data/raw/`.

---

## Sources

- **U.S. Census Bureau** — ACS 5-year Data Profile (2023) via API (ZCTA).  
- **Alabama State Department of Education (ALSDE)** — Supporting Data (2024): Accountability, CCR/Grad Rate, Educator Experience, Proficiency.  
- **Zillow Research** — ZHVI (ZIP-level), latest month labeled on chart.

---

## Notes & disclaimers

- ZCTA/ACS comparisons use **35091 / 35116 / 35180** and may extend beyond exact city limits.  
- Public datasets are occasionally revised; charts reflect the latest available at run time.  
- This repo aims to share public facts with sources. Corrections or improvements are welcome via **Issues/PRs**.  
- Intended as non-partisan civic information.

---

## License

- **Code & derived CSVs:** MIT  
- **Charts & text:** CC BY 4.0  
- **Raw source data:** Terms of the original providers apply.
