# Service Accessibility Optimization for Refugee and Host Communities in Jordan

An Equity-Weighted Dual-Community Framework | IEEE CIS AI Research Challenge 2026 | Team The Incredibles

| | |
|---|---|
| **Paper** | [Read the final paper (PDF)](IEEE_Final_Paper_Nahla_Bader_Noor_Alyazouri.pdf) |
| **Code** | [Notebooks](notebooks/), [data](data/), [results](outputs/) |

---
# Service Accessibility Optimization for Refugee and Host Communities in Jordan

**IEEE CIS AI Research Challenge 2026 — Phase 2 Final Submission**  
**Track:** AI for Social Good / Humanitarian Systems  
**Team:** The Incredibles  

---

## Overview

This repository contains the complete implementation of an equity-weighted 
dual-community service accessibility optimization framework for Jordan, 
developed for the IEEE CIS AI Research Challenge 2026.

Jordan hosts over 730,000 registered refugees alongside a strained host 
population, creating acute spatial mismatches between humanitarian service 
facilities and the communities they serve. This research addresses that gap 
using a two-stage computational pipeline combining spatial machine learning 
with discrete optimization.

---

## Primary Contributions

**Contribution 1 — Network-Distance MCLP:**  
We replace standard Euclidean coverage approximations with road circuity-adjusted 
network distances, demonstrating that Euclidean planning underestimates true 
travel distances by up to 60% in camp-hosting governorates (Mafraq, Azraq).

**Contribution 2 — Dual-Community Equity Weighting:**  
We introduce a parameterised equity weight α that explicitly models the 
coverage trade-off between refugee and host populations as a policy-tunable 
variable. No prior humanitarian location-allocation paper has produced this 
trade-off curve for a dual-population context in Jordan.

---

## Methodology

### Stage 1 — K-Means Spatial Clustering
- Groups 610 population demand points into optimal demand zones
- Optimal k selected via Silhouette Score and Davies-Bouldin Index
- Features: latitude, longitude, log(population), vulnerability score

### Stage 2 — Equity-Weighted MCLP
**Objective:** Maximise Σᵢ (α · refugee_popᵢ + (1−α) · host_popᵢ) · yᵢ

**Subject to:**
- Σⱼ∈N(i) xⱼ ≥ yᵢ ∀i — coverage constraint
- Σⱼ xⱼ ≤ p — budget constraint (p = 3 facilities)
- xⱼ, yᵢ ∈ {0,1} — binary constraint

**Evaluated across:**
- Coverage radii S ∈ {2km, 5km, 10km}
- Equity parameter α ∈ {0.3, 0.5, 0.7}
- Distance models: Network (circuity-adjusted) and Euclidean
- Total configurations: 18 solver runs

---

## Key Results

| Configuration | Coverage | Refugee Coverage | Host Coverage |
|---|---|---|---|
| Network, 10km, α=0.3 | **90.48%** | 81.96% | 92.88% |
| Network, 10km, α=0.5 | — | — | — |
| Network, 10km, α=0.7 | — | — | — |

**Finding 1:** Network-distance MCLP achieves 90.48% total population 
coverage with 3 facilities at 10km radius.

**Finding 2:** Camp-hosting governorates (Mafraq, Azraq) exhibit road 
circuity factors of 1.55–1.60×, meaning Euclidean planning underestimates 
true travel distances by up to 60% in the most vulnerable regions.

**Finding 3:** At α=0.3 (host-weighted), refugee coverage (81.96%) trails 
host coverage (92.88%) by 10.92 percentage points — demonstrating that 
coverage-maximising models structurally under-serve refugees without 
explicit equity enforcement.

---

## Repository Structure

jordan_research/
├── data/
│ ├── generate_data.py # synthetic data generation (UNHCR-calibrated)
│ ├── jordan_population.csv # 610 weighted demand-point centroids
│ ├── candidate_sites.csv # 128 candidate facility locations
│ └── governorate_summary.csv # governorate-level statistics (Table 1)
├── notebooks/
│ ├── 01_kmeans_clustering.ipynb # Stage 1: demand zone delineation
│ ├── 02_network_analysis.ipynb # network vs Euclidean distance analysis
│ └── 03_mclp_optimisation.ipynb # Stage 2: MCLP solver (18 configurations)
├── outputs/
│ ├── figures/ # all 7 publication-ready figures
│ ├── mclp_results_all.csv # full 18-run results table
│ ├── cluster_summary.csv # demand zone centroids
│ └── coverage_matrix_stats.csv # coverage statistics by radius
├── paper/ # IEEE-format final paper (PDF)
├── requirements.txt # Python dependencies
└── README.md


---

## How to Reproduce

### 1. Install dependencies
```bash
pip install -r requirements.txt
```

### 2. Generate the dataset
```bash
python3 data/generate_data.py
```

### 3. Run notebooks in order

notebooks/01_kmeans_clustering.ipynb
notebooks/02_network_analysis.ipynb
notebooks/03_mclp_optimisation.ipynb


All outputs and figures are generated automatically.

---

## Data Sources

| Source | Use |
|---|---|
| UNHCR Jordan ODP (2024) — https://data.unhcr.org/en/country/jor | Population statistics for synthetic data calibration |
| Jordan Dept of Statistics — https://dosweb.dos.gov.jo | Host community population figures |
| Giacomin & Levinson (2015), Environment and Planning B | Road circuity factor validation |
| OpenStreetMap contributors | Candidate site geographic reference |

**Note on synthetic data:** Sub-district microdata is subject to UNHCR 
data-sharing protocols. Demand-point coordinates were generated via 
Dirichlet-weighted disaggregation of published governorate-level statistics, 
consistent with established practice in humanitarian spatial analysis.

---

## Dependencies

- Python 3.9+
- pandas, numpy, scikit-learn
- matplotlib, seaborn, folium
- geopandas, shapely
- pulp (ILP solver with CBC backend)
- osmnx, networkx

---

## Citation

If you use this work, please cite:

> [Team: The Incredibles], "Service Accessibility Optimization for Refugee 
> and Host Communities in Jordan: An Equity-Weighted Dual-Community Framework," 
> IEEE CIS AI Research Challenge 2026, Phase 2 Final Paper.

---

## License

MIT License — open source for humanitarian research use.

---

*Submitted to the IEEE CIS AI Research Challenge 2026*  
*Supervisor: [Saleh Altakrouri, Applied Science Private University]*
