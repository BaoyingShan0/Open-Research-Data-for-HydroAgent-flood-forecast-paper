# Community HydroAgent — Open Research Data Package

Companion data, intermediate results, figures, and reproduction scripts (Python) for the manuscript:

> **Community HydroAgent: Formalizing Forecaster Expertise into Skill-Orchestrated Flood Forecasting Workflows**
> Preprint on https://arxiv.org/abs/2607.23983
> Submitted to *AGU Advances*, 2026.05; revised 2026.10.

Distributed under FAIR principles: **F**indable (Zenodo DOI), **A**ccessible (open formats), **I**nteroperable (published schemas), **R**eusable (CC-BY-4.0 / MIT).

---

## What is in this package

This package contains research artefacts — everything needed to inspect, audit, and reproduce the figures and tables in the paper. It also contains the event prompts of the 14-event validation run (`07_workflow_specification/`) and a Windows program, `05_scripts/replay/cha_flood_replay.exe`, that re-runs the two deterministic decision rules — Step 1 analogue scoring and Step 2 flood-type selection — on the released data. The skill files, the orchestration runtime, and the source code are **not** included; they are subject to a pending patent application and a software copyright registration.

```
hydro_agent_data_open/
├── 00_schemas/              Pydantic-exported JSON Schemas for every artefact
├── 01_input_data/           Basin profile · 113 historical cases (1995–2019) · 14 validation events (2020–2024)
├── 02_step0_scheme/         XAJ calibrated parameter sets (5-type and global) + calibration metrics
├── 03_benchmark_results/    5-LLM × 5-fold CV · stability (n=10) · Step1→Step2 chain · Step 1 sensitivity and information ablation · ablation of the LLM prior · workflow case · cost table · canonical 2024 hydrographs
├── 04_figures/              Paper figures (Fig 1–6) and SI figures (S1–S4, S7–S8) as PDFs
├── 05_scripts/              Visualization (Python, with pinned dependencies) · replay/cha_flood_replay.exe (Step 1 and Step 2 re-run)
├── 06_tables/               Supplementary tables (S1–S8) as analyzable CSVs
├── 07_workflow_specification/  14 validation-event prompts as sent (Chinese) with English translations
├── 08_supplementary_basins/ Two further CAMELSH basins (11532500, 08189500): case libraries, Step 0, validation events, Step1→Step2 runs
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
python 05_scripts/visualization/Canonical2024Hydrographs/plot_canonical_2024_hydrographs.py  # Fig S2
python 05_scripts/visualization/Ablation/plot_fig5_ablation.py                         # Fig 5
python 05_scripts/visualization/LLMs_kfold_performance_costs.py                       # Fig 6
python 05_scripts/visualization/Step0Comparison/plot_paired_dumbbell.py               # Fig S5
python 05_scripts/visualization/fig_step1_stability.py                                # Fig S7
python 05_scripts/visualization/fig_step1_part_2.py --llms deepseek chatgpt-5.4 gemini qwen-3.6-plus claude-4.6      # Fig S6

# 3. Re-run the deterministic Step 1 retrieval and Step 2 selection for the 14 validation events
#    (Windows 10/11, 64-bit; checks the Top-5 analogues and Table S8 KGE; see 05_scripts/replay/README.md)
05_scripts\replay\cha_flood_replay.exe
```

Each script reads from `03_benchmark_results/` and writes the corresponding PDF. The published versions of these PDFs are in `04_figures/` for byte-level comparison.

---

## Limits of reproducibility

This package supports inspection of the released data, benchmark artefacts, tables, and figure-generation scripts. It does not include the orchestration runtime or the live LLM execution environment, so it cannot regenerate LLM calls or benchmark production runs from scratch. The deterministic parts of Steps 1 and 2 can be re-run with `05_scripts/replay/cha_flood_replay.exe`, and the prompts the LLM received are in `07_workflow_specification/`.

LLM outputs are non-deterministic, and closed-source model versions may change over time. The **References**`03_benchmark_results/step1_stability_gpt_n10/` directory is included to quantify this variability for the cited GPT-5.4 Step 1 runs.

---

## Data provenance


| Source                                                                                    | This package                                                                 | Upstream                                                                                                        |
| ----------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| Hourly hydrometeorological forcing (1995–2024)                                            | `01_input_data/basin_14194150/`                                              | CAMELSH dataset — Tran et al. (2025), Zenodo [16763144](https://zenodo.org/records/16763144)                    |
| Streamflow observations                                                                   | embedded in event JSONs                                                      | USGS gauge 14194150, South Yamhill River at McMinnville, OR ([waterdata.usgs.gov](https://waterdata.usgs.gov/)) |
| 113 historical flood events + 14 validation events                                        | `01_input_data/historical_cases_113/`, `01_input_data/validation_events_14/` | Derived in this study (peak-over-threshold extraction)                                                          |
| XAJ calibrated parameter sets                                                             | `02_step0_scheme/`                                                           | Calibrated in this study using DDS (Tolson & Shoemaker, 2007)                                                   |
| K-fold CV, stability, Step1→Step2 benchmark results, Step 1 sensitivity and information ablation, ablation of the LLM prior, and canonical 2024 hydrograph inputs | `03_benchmark_results/`                                                      | Generated in this study                                                                                         |
| Supplementary basins: case libraries, Step 0 schemes, validation events, benchmark runs | `08_supplementary_basins/`                                                   | Derived in this study from CAMELSH and USGS gauges 11532500 (Smith River near Crescent City, CA) and 08189500 (Mission River at Refugio, TX) |


---

## Citation

If you use these data or scripts, please cite:

1. **This paper** (preferred, supersedes the dataset DOI):
  Community HydroAgent: Formalizing Forecaster Expertise into Skill-Orchestrated Flood Forecasting Workflows. *AGU Advances*, submitted in 2026.
2. **This dataset**: Zenodo concept DOI [10.5281/zenodo.20644504](https://doi.org/10.5281/zenodo.20644504), which resolves to the latest version (v1.0.0: 10.5281/zenodo.20644505) — see `CITATION.cff`.
3. **Upstream CAMELSH dataset**: Tran et al. (2025), Zenodo [10.5281/zenodo.16763144](https://doi.org/10.5281/zenodo.16763144).

---

## Licenses

- **Data** (everything under `01_input_data/`, `02_step0_scheme/`, `03_benchmark_results/`, `06_tables/`, `07_workflow_specification/`, `08_supplementary_basins/`): Creative Commons Attribution 4.0 International ([CC-BY-4.0](LICENSE-DATA.txt)).
- **Scripts** (everything under `05_scripts/` and the helper scripts at this level): MIT License ([MIT](LICENSE-CODE.txt)).
- **Exception:** `05_scripts/replay/cha_flood_replay.exe` is distributed under [`05_scripts/replay/TERMS.txt`](05_scripts/replay/TERMS.txt), not under MIT.

Derived figures in `04_figures/` inherit the data license (CC-BY-4.0).

---

## Contact

- Corresponding author: Baoying Shan, [baoyingshan0@163.com](baoyingshan0@163.com)
- Issues, errata, and reuse questions: contact the corresponding author.

---

## Version

See `[CHANGELOG.md](CHANGELOG.md)`. This README documents v1.1.0, which adds the material of the first-round revision of the AGU Advances submission to the initial release (v1.0.0).