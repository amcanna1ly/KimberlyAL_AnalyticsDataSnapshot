# Kimberly, AL — Public Data Snapshot (v1)

Neutral, replicable charts built from public sources so neighbors can see Kimberly’s trajectory.

**Focus (v1):**
- **Housing:** ZIP 35091 (plus 35116/35180 for context) — Zillow ZHVI 2000–2025  
- **Education (Mortimer Jordan HS):** Graduation, College & Career Readiness (CCR), Academic Growth, Educator Experience — ALSDE 2024  
- **Next:** ACS demographics (planned for v2)

**Outputs:** high-resolution PNG charts + small, shareable CSVs (derived).

_Last updated: Aug 21, 2025_


---

## Quickstart (≈5 min)

1) **Create & activate a clean Python env**
```bash
# Windows (PowerShell)
py -m venv .venv
. .venv/Scripts/Activate.ps1
pip install -r requirements.txt
```

2) **Launch Jupyter**
```bash
pip install jupyter
jupyter notebook
```

3) **Open & run notebooks**
- `notebooks/Kimberly_HomeValuations.ipynb`
- `notebooks/Kimberly_EduStats.ipynb`

The first cell in each notebook creates folders and sets paths.  
Place source CSVs in `data/raw/` (see **Data you need** below), then **Run All**.  
Charts land in `charts/` and derived CSVs in `data/processed/`.


---

## Repo layout

```
kimberly-public-data-snapshot/
├─ README.md
├─ requirements.txt
├─ .gitignore
├─ charts/                # generated PNGs (safe to commit)
├─ data/
├─ notebooks/
│  ├─ Kimberly_HomeValuations.ipynb
│  └─ Kimberly_EduStats.ipynb
└─ outputs/               # optional PDFs/HTML exports
```

---

## Data you need

### Housing — Zillow ZHVI (ZIP level)
- File(s): ZIP-level ZHVI monthly time series (typical home value)  
- ZIPs: **35091** (Kimberly), **35116** (Morris), **35180** (Warrior)  
- Note: ZIP areas ≠ city boundaries; used here for market-trend context.

### Education — ALSDE Supporting Data (2024)
CSV exports for:
- **Accountability** (includes **Graduation Rate**, **College & Career Readiness**, **Academic Growth**, **Chronic Absenteeism**)  
- **Educator Credentials/Experienced** (experienced vs novice/total)  
- *(Optional)* CCR/Grad Rate file (not required since Accountability already includes Grad/CCR)

**Guardrail:** If an export shows `SystemCode=000 / SchoolCode=0000`, it’s **statewide-only** and will not include MJHS rows. Re-export either:
- **All Systems / All Schools** (then filter in the notebook), or  
- **System = Jefferson County** and **School = Mortimer Jordan HS**.

**Tip (Windows paths):** use forward slashes (`C:/Users/...`) or raw strings (`r"C:\Users\..."`).


---

## Notebook behavior

### `notebooks/Kimberly_HomeValuations.ipynb`
- Loads Zillow ZHVI ZIP-level CSV(s) from `data/raw/`
- Builds tidy monthly time series for selected ZIPs (35091/35116/35180)
- **Chart:** `charts/home_values_35091_35116_35180_2000_2025.png`  
  Footer includes source/month and notes “ZIP-level (not city boundary)”

### `notebooks/Kimberly_EduStats.ipynb`
- Loads **Accountability** (+ optional CCR/Grad) and **Educator** CSVs from `data/raw/`
- Filters to **System = Jefferson County** & **School = Mortimer Jordan High School** and **All** groups (All Grade/Gender/Race/Ethnicity/Sub Pop)
- **Charts:**
  - `charts/mjhs_grad_ccr_2024.png` (Graduation & CCR)
  - `charts/mjhs_educator_experience_2024.png` (experienced vs novice)
- **Derived CSVs** saved to `data/processed/` for sharing

**Built-in guardrail:** If a file is statewide-only (000/0000), the notebook prints a clear instruction to re-export.


---

## Bootstrap cell

```python
# Cell 0 — bootstrap folders & paths (safe to re-run)
from pathlib import Path
import shutil

ROOT = Path.cwd().resolve()
DATA_RAW = ROOT / "data" / "raw"
DATA_PROCESSED = ROOT / "data" / "processed"
CHARTS = ROOT / "charts"
OUTPUTS = ROOT / "outputs"

for p in [DATA_RAW, DATA_PROCESSED, CHARTS, OUTPUTS]:
    p.mkdir(parents=True, exist_ok=True)

# Optional: clean previous outputs for a fresh build
# for p in [DATA_PROCESSED, CHARTS, OUTPUTS]:
#     for f in p.glob("*"):
#         try: f.unlink()
#         except IsADirectoryError: shutil.rmtree(f)
```

### Small helper to stamp chart footers
```python
def add_footer(ax, text):
    ax.figure.text(0.01, 0.01, text, ha="left", va="bottom", fontsize=9, alpha=0.8)

# Example:
# add_footer(plt.gca(), "Source: Zillow ZHVI • Jul 2025 • ZIP-level (35091/35116/35180)")
```


---

## Reproducibility

- **requirements.txt**
  ```
  pandas
  matplotlib
  jupyter
  ```
- Each chart shows **source + date** in the footer.  
- Notebooks write **processed CSVs** so reviewers can check numbers without large raw files.  
- Commit history documents any updates/revisions to methods/inputs.


---

## Notes & disclaimers

- **ZIP vs City:** Housing trends use ZIPs (35091/35116/35180). ZIPs may extend beyond city limits.  
- **Statewide vs School files:** ALSDE statewide-only exports don’t include MJHS rows; export All Systems/Schools or Jefferson County/MJHS.  
- **Estimates:** Public datasets can update and revise prior periods; charts reflect the latest available at run time.  
- **Neutral framing:** This repository shares public facts with sources; corrections or additional sources are welcome via issues/PRs.


---

## Sources

- **Zillow Research** — ZHVI (ZIP level), latest month labeled on chart  
- **Alabama State Department of Education** — Supporting Data (2024), “All” groups (Accountability; Educator)  
- **(Planned)** U.S. Census Bureau — ACS 5-year (DP03/DP04/DP05)


---

## License

- **Code & derived CSVs:** MIT  
- **Charts & README text:** CC BY 4.0  
- **Raw datasets:** Follow original source terms; raw files live in `data/raw/` and are **not** committed.


---

## Contact

Issues and pull requests welcome.

**Alex McAnnally**  
amcannally.cloud • 205-936-5024
