# HydroAgent — Open Research Data Package

Companion data, intermediate results, figures, and reproduction scripts (Python) for the manuscript:

> **HydroAgent: Formalizing Forecaster Expertise into Skill-Orchestrated Flood Forecasting Workflows**
> Submitted to *AGU Advances*, 2026.05.

Distributed under FAIR principles: **F**indable (Zenodo DOI), **A**ccessible (open formats), **I**nteroperable (published schemas), **R**eusable (CC-BY-4.0 / MIT).

---

## What is in this package

This package contains *research artefacts only* — everything needed to inspect, audit, and reproduce the figures and tables in the paper. The HydroAgent codebase itself (skills, contracts, adapters, calibration engine) is **not** included in this data package.

```
hydro_agent_data_open/
├── 00_schemas/              Pydantic-exported JSON Schemas for every artefact
├── 01_input_data/           Basin profile · 113 historical cases (1995–2019) · 14 validation events (2020–2024)
├── 02_step0_scheme/         XAJ calibrated parameter sets (5-type and global) + calibration metrics
├── 03_benchmark_results/    5-LLM × 5-fold CV · stability (n=10) · Step1→Step2 chain · workflow case · cost table · canonical 2024 hydrographs
├── 04_figures/              Paper figures (Fig 1–5) and SI figures (S1–S4) as PDFs
├── 05_scripts/              Visualization (Python, with pinned dependencies)
├── 06_tables/               Supplementary tables as analyzable CSVs
├── MANIFEST.md              File-level inventory with provenance pointers
├── CITATION.cff             Machine-readable citation
├── metadata.yml             Zenodo deposition metadata
├── LICENSE-DATA.txt         Creative Commons Attribution 4.0 (CC-BY-4.0)
└── LICENSE-CODE.txt         MIT License
```

See `[MANIFEST.md](MANIFEST.md)` for the full file-level inventory.

---

## Reproducing the paper figures

```bash
# 1. Set up environment
#    Run these commands from the hydro_agent_data_open folder.
pwd
cd /path/to/hydro_agent_data_open
#conda create -n hydro_agent_data_open python=3.10.11
#conda activate hydro_agent_data_open
pip install -r 05_scripts/requirements.txt

# 2. Reproduce each figure (output → 04_figures/)
python 05_scripts/visualization/fig_step1_combined.py                                  # Fig 3
python 05_scripts/visualization/EventsHydrograph/plot_selected_event_hydrographs.py   # Fig 4
python 05_scripts/visualization/Canonical2024Hydrographs/plot_canonical_2024_hydrographs.py  # Fig S1
python 05_scripts/visualization/LLMs_kfold_performance_costs.py                       # Fig 5
python 05_scripts/visualization/Step0Comparison/plot_paired_dumbbell.py               # Fig S2
python 05_scripts/visualization/fig_step1_stability.py                                # Fig S3
python 05_scripts/visualization/fig_step1_part_2.py --llms deepseek chatgpt-5.4 gemini qwen-3.6-plus claude-4.6      # Fig S4

```

Each script reads from `03_benchmark_results/` and writes the corresponding PDF. The published versions of these PDFs are in `04_figures/` for byte-level comparison.

---

## Limits of reproducibility

This package supports inspection of the released data, benchmark artefacts, tables, and figure-generation scripts. It does not include the HydroAgent codebase or live LLM execution environment, so it cannot fully regenerate LLM calls or benchmark production runs from scratch.

LLM outputs are non-deterministic, and closed-source model versions may change over time. The **References**`03_benchmark_results/step1_stability_gpt_n10/` directory is included to quantify this variability for the cited GPT-5.4 Step 1 runs.

---

## Data provenance


| Source                                                                                    | This package                                                                 | Upstream                                                                                                        |
| ----------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| Hourly hydrometeorological forcing (1995–2024)                                            | `01_input_data/basin_14194150/`                                              | CAMELSH dataset — Tran et al. (2025), Zenodo [16763144](https://zenodo.org/records/16763144)                    |
| Streamflow observations                                                                   | embedded in event JSONs                                                      | USGS gauge 14194150, South Yamhill River at McMinnville, OR ([waterdata.usgs.gov](https://waterdata.usgs.gov/)) |
| 113 historical flood events + 14 validation events                                        | `01_input_data/historical_cases_113/`, `01_input_data/validation_events_14/` | Derived in this study (peak-over-threshold extraction)                                                          |
| XAJ calibrated parameter sets                                                             | `02_step0_scheme/`                                                           | Calibrated in this study using DDS (Tolson & Shoemaker, 2007)                                                   |
| K-fold CV, stability, Step1→Step2 benchmark results, and canonical 2024 hydrograph inputs | `03_benchmark_results/`                                                      | Generated in this study                                                                                         |


---

## Citation

If you use these data or scripts, please cite:

1. **This paper** (preferred, supersedes the dataset DOI):
  HydroAgent: Formalizing Forecaster Expertise into Skill-Orchestrated Flood Forecasting Workflows. *AGU Advances*, submitted in 2026.
2. **This dataset**: Zenodo DOI `10.5281/zenodo.20304955` (assigned on upload — see `CITATION.cff`).
3. **Upstream CAMELSH dataset**: Tran et al. (2025), Zenodo [10.5281/zenodo.16763144](https://doi.org/10.5281/zenodo.16763144).

---

## Licenses

- **Data** (everything under `01_input_data/`, `02_step0_scheme/`, `03_benchmark_results/`, `06_tables/`): Creative Commons Attribution 4.0 International ([CC-BY-4.0](LICENSE-DATA.txt)).
- **Scripts** (everything under `05_scripts/` and the helper scripts at this level): MIT License ([MIT](LICENSE-CODE.txt)).

Derived figures in `04_figures/` inherit the data license (CC-BY-4.0).

---

## Contact

- Corresponding author: Baoying Shan, [baoying.shan@polimi.it](mailto:baoying.shan@polimi.it)
- Issues, errata, and reuse questions: contact the corresponding author.

---

## Version

See `[CHANGELOG.md](CHANGELOG.md)`. This README documents the initial release (v1.0) prepared alongside the AGU Advances submission.