# CODEBOOK — Simulation Output Variables

This codebook documents the columns in `outputs_simulation/sim_main.csv`
and the summary tables in `inputs_simulation/`.

## sim_main.csv — Cell-level raw results (7,200 rows)

### Design factors
| Column | Description |
|--------|-------------|
| `N` | Training sample size (200, 500, 1000) |
| `lambda` | Standardized loading (0.70, 0.85) |
| `rho` | Correlation between Q and V residuals (0.0, 0.4) |
| `truth` | True DGP model ID (M0, M6, M7) |
| `rep` | Replication index (1–200) |
| `status` | "ok" or error message |

### Naïve CV pipeline
| Column | Description |
|--------|-------------|
| `naive_model` | Model selected by naïve 10-fold CV |
| `naive_reported_rmse` | RMSE reported by naïve CV (optimistically biased) |
| `naive_true_rmse` | True RMSE on independent test set (N_test = 5,000) |
| `naive_true_mae` | True MAE on independent test set |
| `naive_optimism_rmse` | Miscalibration: reported − true RMSE |
| `naive_TPR` | True positive rate for optional edges |
| `naive_FPR` | False positive rate for optional edges |
| `naive_F1` | F1 score for optional edges |

### Nested CV pipeline — Primary (frequency-weighted)
| Column | Description |
|--------|-------------|
| `nested_top_model` | Most frequently selected model across outer folds |
| `nested_reported_rmse` | RMSE reported by nested CV outer loop (unbiased) |
| `nested_true_rmse` | Frequency-weighted true RMSE on independent test set |
| `nested_true_mae` | Frequency-weighted true MAE on independent test set |
| `nested_optimism_rmse` | Miscalibration: reported − frequency-weighted true RMSE |
| `nested_TPR` | Per-fold averaged TPR across outer folds |
| `nested_FPR` | Per-fold averaged FPR across outer folds |
| `nested_F1` | Per-fold averaged F1 across outer folds |

### Nested CV pipeline — Secondary (modal-model, for robustness)
| Column | Description |
|--------|-------------|
| `nested_modal_true_rmse` | True RMSE of the single most-frequent model |
| `nested_modal_true_mae` | True MAE of the single most-frequent model |
| `nested_modal_optimism` | Miscalibration: reported − modal true RMSE |
| `nested_modal_TPR` | TPR of the single most-frequent model |
| `nested_modal_FPR` | FPR of the single most-frequent model |
| `nested_modal_F1` | F1 of the single most-frequent model |

### Notes on evaluation paradigm
- **Primary metrics** use frequency-weighted evaluation: each uniquely
  selected model is fit on the full training set and evaluated on the
  independent test set, then weighted by its selection frequency. This
  matches the empirical paradigm where different outer folds may select
  different models.
- **Secondary (_modal_) metrics** evaluate only the single most-frequently
  selected model, which matches the "deploy the winner" paradigm. These
  are retained for backward compatibility and robustness comparison.

## Table_S2_optimism.csv — Miscalibration summary

Aggregated means of optimism (reported − true RMSE) and true performance
by design cell (N × lambda × rho × truth).

## Table_S3_selection_accuracy.csv — Selection accuracy summary

Aggregated means of TPR, FPR, F1 for both naïve and nested pipelines
by design cell.

## Table_S4_stable_core_tau.csv — Stability-augmented model selection

True RMSE/MAE and inclusion probabilities (pE, pQ, pV) for the
stable-core model at each stability threshold τ ∈ {0.6, 0.7, 0.8},
aggregated by design cell × τ × core_model.
