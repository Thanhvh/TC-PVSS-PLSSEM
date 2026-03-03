# Supplementary Materials — Reproducibility Package (FULL)

Created: 2026-03-02

## Quick start (rebuild figures without rerunning simulation)

1) Rebuild MAIN figures (Figures 1–5):
```r
setwd("code_figures")
source("99_session_info.R")
source("01_make_figures_main.R")
```

2) Rebuild SIMULATION figures (Figures S1–S4):
```r
setwd("code_figures")
source("99_session_info.R")
source("02_make_figures_simulation.R")
```

## (Optional) Rerun the FULL simulation

From the `code_simulation/` directory:
```r
source("00_config.R")
source("01_core_functions.R")
source("02_run_simulation_parallel.R")
source("03_summarize_simulation.R")
```

Expected runtime: ~2h 54min on 24 cores (7,200 design cells).

## Contents

| Folder | Description |
|--------|-------------|
| `code_simulation/` | Simulation source code (4 R scripts) |
| `code_figures/` | Figure reproduction code (base R) |
| `inputs_main/` | CSV inputs for main figures (from empirical ACSI pipeline) |
| `inputs_simulation/` | Summary tables from simulation (Tables S2–S4) |
| `outputs_simulation/` | Raw simulation output (`sim_main.csv`; see CODEBOOK.md) |
| `logs/` | Run log from the FULL simulation (timestamps + parameters) |

## Design

The simulation implements a full-factorial Monte Carlo study with:
- 3 sample sizes (N = 200, 500, 1000)
- 2 loading levels (λ = 0.70, 0.85)
- 2 correlation levels (ρ = 0.0, 0.4)
- 3 true DGPs (M0, M6, M7)
- 200 replications per cell
- Total: 3 × 2 × 2 × 3 × 200 = 7,200 simulation cells

Naïve and nested CV both use 10-fold CV for the evaluation/selection loop,
ensuring any performance differences are attributable to the nesting
structure rather than resampling-budget asymmetry.

The loyalty construct (L) is measured by 3 reflective indicators
(REPUR, HIGHPTOL, LOWPTOL), matching the empirical ACSI illustration.
