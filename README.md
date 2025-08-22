Kimberly, AL — Public Data Snapshot
=================================================

Neutral, replicable charts from public datasets so neighbors can see Kimberly’s trajectory.

What’s inside
-------------
- Housing (Zillow ZHVI, ZIP level): 35091 with 35116/35180 for context (2000–2025).
- Education (ALSDE 2024): Mortimer Jordan HS — Graduation, College & Career Readiness (CCR), Academic Growth, Educator Experience.
- Demographics & Economics (ACS 5-yr 2023 via API): Population, age mix, median household income, labor force %, unemployment %, owner-occupied %, median home value, median rent for ZCTAs 35091/35116/35180.

Outputs: small CSVs + high-resolution PNG charts, ready for posts and slides.

Last updated: Aug 22, 2025

Quickstart
----------
1) Setup
   - macOS/Linux
     python3 -m venv .venv && source .venv/bin/activate
     pip install -r requirements.txt

   - Windows PowerShell
     py -m venv .venv
     . .venv/Scripts/Activate.ps1
     pip install -r requirements.txt

   Optional: set a Census API key to avoid rate limits
     macOS/Linux: export CENSUS_KEY=YOUR_KEY_HERE
     Windows PS:  $env:CENSUS_KEY="YOUR_KEY_HERE"

2) Generate ACS ZIP snapshot (CSV + charts)
   python acs_zip_snapshot.py --zips 35091 35116 35180 --labels "Kimberly (35091),Morris (35116),Warrior (35180)" --outdir charts

3) Open notebooks (optional)
   pip install jupyter
   jupyter notebook
   - notebooks/Kimberly_HomeValuations.ipynb
   - notebooks/Kimberly_EduStats.ipynb

Repository layout
-----------------
KimberlyAL_AnalyticsDataSnapshot/
├─ README.md
├─ requirements.txt
├─ acs_zip_snapshot.py          # ACS API script (creates CSVs + PNGs)
├─ charts/                      # generated outputs (safe to commit)
├─ data/
│  ├─ raw/                      # optional if using flat files
│  └─ processed/                # tidy CSVs written by notebooks
└─ notebooks/
   ├─ Kimberly_HomeValuations.ipynb
   └─ Kimberly_EduStats.ipynb

ACS ZIP snapshot (what it does)
-------------------------------
Fetches ACS 5-year Data Profile metrics for ZCTAs and writes:
- CSV: acs_{YEAR}_profile_ZCTA_35091_35116_35180.csv
- Charts:
  - acs_pop.png — Total population
  - acs_income.png — Median household income
  - acs_home_value.png — Median home value (owner-occupied)
  - acs_unemployment.png — Unemployment rate

Labels on charts use friendly names (e.g., “Kimberly (35091)”).
Note: ZCTAs are the Census ZIP-based areas and may not match exact city limits.

Optional:
  python acs_zip_snapshot.py --include-decennial-growth
  (also fetches 2010→2020 Decennial population change and charts it)

Notebooks (summary)
-------------------
Kimberly_HomeValuations.ipynb
- Loads Zillow ZHVI ZIP-level data and builds a 2000–2025 line chart for 35091/35116/35180.
- Figure notes clarify “ZIP-level (not city boundary).”

Kimberly_EduStats.ipynb
- Uses ALSDE Supporting Data (2024). Filters to System = Jefferson County and School = Mortimer Jordan High School, “All” groups.
- Produces:
  - mjhs_grad_ccr_2024.png — Graduation & CCR
  - mjhs_educator_experience_2024.png — Experienced vs novice staff
- Guardrail: if an ALSDE export is statewide-only (000/0000), the notebook prompts to re-export with district/school rows.

Reproducibility
---------------
requirements.txt
- pandas
- requests
- matplotlib
- jupyter

- All charts are generated from code; CSVs are committed for verification.
- Each chart includes a clear title; data vintage is noted in the README and commits.

Sources
-------
- Zillow Research — ZHVI (ZIP level), latest month labeled on chart.
- Alabama State Department of Education — Supporting Data (2024), “All” groups for MJHS.
- U.S. Census Bureau — ACS 5-year Data Profile (2023) via API (ZCTA).
- U.S. Census Bureau — Decennial Census 2010 & 2020 (optional growth add-on).

Notes & disclaimers
-------------------
- ZIP/ACS comparisons use ZCTAs (35091/35116/35180); these can extend beyond city limits.
- Public datasets occasionally revise prior periods; charts reflect the latest available at run time.
- This repo aims to share public facts with sources. Corrections or improvements are welcome via Issues/PRs.

License
-------
- Code & derived CSVs: MIT
- Charts & text: CC BY 4.0
- Raw source data follows original provider terms.
