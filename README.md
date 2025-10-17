# U.S. vs Washington State: Trends and County‑Level Analysis of Electric Vehicle Adoption

A reproducible analysis notebook comparing **EV adoption in Washington State vs. the United States**, with county‑level drill‑downs, policy context, and investment guidance. The project cleans two public datasets, engineers features (e.g., **BEV share**, **growth volatility**, **CAFV eligibility**), and produces clear, decision‑oriented visualizations.

> **Notebook**: `A09-US-vs-WA-EV-Adoption (1).ipynb`

---

## Table of Contents
- [Executive Summary](#executive-summary)
- [Project Goals](#project-goals)
- [Data Sources](#data-sources)
- [Project Structure](#project-structure)
- [Environment & Dependencies](#environment--dependencies)
- [Reproducibility: How to Run](#reproducibility-how-to-run)
- [Data Preparation](#data-preparation)
- [Analysis & Visualizations](#analysis--visualizations)
- [Key Findings](#key-findings)
- [Limitations](#limitations)
- [Future Work](#future-work)
- [References](#references)
- [License](#license)

---

## Executive Summary

This project assesses how Washington’s EV adoption compares to national trends and explores **county‑level dynamics** across market composition, geographic stability, and behavioral drivers. It uses engineered indicators (e.g., **BEV share over time**, **growth volatility**, **average BEV range**, **CAFV eligibility**) to surface where adoption is accelerating, stable, or lagging, and which factors are most associated with higher uptake.

**At a glance:**
- Washington is among the top states by EV penetration, with **faster BEV growth than PHEV**, indicating improving range and consumer confidence.
- Adoption is **uneven across counties**; urban centers and large‑utility service areas lead, while rural areas lag.
- **Tesla, Nissan, Chevrolet** dominate WA’s EV market share.
- **CAFV policy incentives** helped shift the market toward full battery‑electric models.
- With continued **infrastructure expansion in rural counties**, Washington appears **on track** for 2030 targets.

---

## Project Goals
- **EV Landscape Overview**: Compare EV adoption across U.S. states with a Washington focus.
- **Adoption Growth & County Trends**: Track year‑over‑year adoption (2017–2025), spotlight stability vs. volatility.
- **Drivers of Adoption**: Examine **average BEV range**, **CAFV eligibility**, and **utility coverage**.
- **Readiness & Policy Guidance**: Provide **investment and policy insights** for accelerating EV adoption.

---

## Data Sources

> The notebook reads two CSVs from Google Drive. Place them locally in a `data/` folder for reproducibility (see **Reproducibility**).

1. **Electric_Vehicle_Population_Size_History_By_County.csv**  
   - Long‑format historical counts by **county** and **year** (focus: WA; used for growth/stability, heatmaps).
2. **Electric_Vehicle_Population_Data.csv**  
   - Vehicle‑level/aggregated attributes (e.g., **make**, **model**, **BEV/PHEV**, **CAFV eligibility**, **range**, **utility**); used to build market composition, range, and policy features.

> These datasets are commonly published by the **Washington State Department of Licensing / WA Open Data**. If you obtained updated versions, keep the same filenames.

---

## Project Structure

```
.
├─ A09-US-vs-WA-EV-Adoption (1).ipynb   # Main analysis notebook
└─ data/
   ├─ Electric_Vehicle_Population_Size_History_By_County.csv
   └─ Electric_Vehicle_Population_Data.csv
```

---

## Environment & Dependencies

- Python 3.9+
- Core: `pandas`, `numpy`
- Viz: `matplotlib`, `seaborn`
- QA/EDA: `missingno`
- (Colab) `google.colab` for Drive mounting (optional)

Install locally:

```bash
pip install pandas numpy matplotlib seaborn missingno
```

---

## Reproducibility: How to Run

**Option A — Google Colab**
1. Upload the notebook to Colab.
2. Mount Google Drive (if using Drive paths) or upload the two CSVs to the Colab session.
3. Update the two `read_csv(...)` paths to point to your files (or place them in `/content/data/` and use the paths below).

**Option B — Local (VS Code / Jupyter)**
1. Clone/download this project and create a `data/` folder in the same directory as the notebook.
2. Place both CSVs in `./data/`.
3. In the notebook, set:
    ```python
    HIST_PATH = "data/Electric_Vehicle_Population_Size_History_By_County.csv"
    POP_PATH  = "data/Electric_Vehicle_Population_Data.csv"
    ```
4. Run cells top‑to‑bottom.

> Tip: If you’re using different filenames, update the constants accordingly.

---

## Data Preparation

The notebook performs the following **cleaning & feature engineering** steps:

- **Standardize identifiers**
  - Title‑case counties, strip whitespace, consistent state labels.
- **Filter & harmonize time snapshots**
  - Aligns on **April 30** snapshots for U.S./WA trend comparability.
- **Construct indicators**
  - `BEV_Share`, `BEV_Share_pct`, `Percent_EV` (EVs / total), `Growth_Std` (adoption volatility), `Avg_Range` (for BEVs).
- **Normalize policy field**
  - `normalize_cafv(...)` converts the CAFV eligibility field into consistent categories.
- **Utility coverage & similarity**
  - Compute **Total_Utility_EV** and a **utility similarity index** (see `utility_similarity(...)`) to compare county adoption patterns across major utilities.
- **Helpers**
  - `pick(...)`, `row_normalize(...)` used for shaping tables and normalizing across comparisons.

---

## Analysis & Visualizations

The notebook generates the following figures (representative titles):

1. **Ratio of Electric Vehicles to Non‑Electric Vehicles Over Time** — EV share trajectory.
2. **U.S. EV Trend (April 30 Each Year, Excluding Washington)** — national baseline.
3. **Washington EV Trend (April 30 Each Year)** — WA trajectory.
4. **BEV Share Over Time (April 30 snapshots)** — BEV vs PHEV dynamics.
5. **BEV vs. PHEV Share – Washington vs. Rest of U.S.** — composition comparison.
6. **Top 10 Car Brands in Washington State** — market leaders.
7. **Top N Washington Counties by EV Adoption (%) — Latest Snapshot** — county‑level adoption leaders.
8. **Top Utility Providers Supporting EVs in Washington** — utility footprint.
9. **Top N Most Volatile Counties (Highest Growth Std)** — stability vs. risk.
10. **Stability vs. Maturity: BEV Share vs EV Growth Volatility (WA Counties)** — scatter of share vs volatility.
11. **Range‑Confidence Hypothesis: Average BEV Range vs. EV Adoption by County** — range related to adoption.
12. **Utility Scale vs. EV Adoption Intensity (Top 10 Utilities)** — capacity vs adoption.
13. **CAFV Eligibility by Model Year (WA registrations)** — policy effects over time.
14. **County EV Adoption Patterns by Utility** — per‑utility facet (suptitle).
15. **EV Investment Priority Matrix for WA Counties** — prioritization grid for action.

> The notebook also includes **heatmaps (2017–2025)** for county‑level fluctuation and stability.

---

## Key Findings

- **WA remains a national leader** in EV adoption but shows **urban–rural gaps**.
- **BEV growth outpaces PHEV**, consistent with improving range and charging confidence.
- **Tesla, Nissan, Chevrolet** dominate WA market share; composition differs from the rest of the U.S.
- Counties served by **large utilities** display **higher and more stable** adoption.
- **CAFV incentives** correlated with growth and a market tilt toward BEVs.
- Washington appears **on a viable trajectory** toward **2030** targets **if** rural infrastructure expands.

---

## Limitations

- **WA‑centric datasets**; national trends are approximated through aggregation and may not reflect all states uniformly.
- **Snapshot alignment** (e.g., April 30) improves comparability but can omit intra‑year variation.
- **CAFV** fields can be **messy**; normalized categories still inherit any source reporting errors.
- **Range data** is vehicle‑reported and may not reflect real‑world usage.

---

## Future Work

- Enrich with **charging station density** (DCFC/L2), **median income**, **commute patterns**, and **housing type**.
- Introduce **spatial models** (e.g., spatial lag/error) for county influence effects.
- Formal **forecasting** (ARIMA/Prophet/XGBoost) for 2030 target tracking.
- Robustness checks with **VIN‑level datasets** and **non‑WA states** for stronger national comparisons.
- **Equity lens**: prioritize investments using demographics & access metrics.

---

## References

- Washington State DOL / WA Open Data: Electric Vehicle Population Data; Electric Vehicle Population Size History by County.
- Additional policy context: Washington State **Clean Alternative Fuel Vehicle (CAFV)** program materials.

> If you used newer/alternate sources, list them here explicitly with links and access dates.

---

## License

If you intend to share this analysis, include a license (e.g., MIT). Otherwise, remove this section.
