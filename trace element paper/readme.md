# Analysis code

R scripts that produce the figures, tables, and statistical results reported in *Reiter et al.*, "Trace element archives in narwhal tusks ...". Each script is named for the topic it covers and the paper section it feeds.

## How to run

All scripts assume `analysis code/` is the working directory. They load data via relative paths from this folder.

`00_DataPreparation_Shared.R` is the shared data loader, sourced at the top of `TiDetectionLimitDiagnostic_Methods.R` and `RegionalAndTrendAnalysis_LOO_MainTable2.R`. The other scripts read CSVs directly.

## Scripts

| Script | Produces |
|---|---|
| `00_DataPreparation_Shared.R` | Shared data loader; applies tip-of-tusk corrections (4076 +10 yr, 938 +7 yr, 956 +2 yr) and excludes Fe, U, Ca. |
| `HeavyElementEvents_Main.R` | Main Fig. `fig:heavy` — synchronous excursions of Pb, Cu, Zn, Mn, Ba across tusks. |
| `WeaningModelFit_MainAndSupplement.R` | Main Fig. `fig:timeofweaning` (16-tusk fit, $t^* = 29.2$ mo) and Supp Fig. `fig:clean_subset_fit` (4 clean-signal tusks). |
| `ElementTrendsByYearAndAge_Main.Rmd` | Main Fig. `fig:ageyearcomp_2reg` — element trends vs year and age, by region. |
| `PCABiplotRegional_Main.R` | Main Fig. `fig:pcareg` — PCA biplots, 15×15 and 16×12 configurations. |
| `RegionalAndTrendAnalysis_LOO_MainTable2.R` | **Full pipeline for Main Table 2.** Runs three tests per element on quarter-year-binned data, with per-tusk medians on raw ppm and per-tusk Theil-Sen slopes on within-tusk standardized signals: (i) East–West permutation on medians, (ii) East–West permutation on slopes, (iii) **two-stage Arctic-wide trend estimator** — per-tusk Theil-Sen slopes (Stage 1) aggregated via Wilcoxon signed-rank vs 0 (Stage 2). All three tests are run on the full window, the 1982–2009 overlap window, and 16 leave-one-tusk-out variants for sensitivity. Writes `loo_table1_all_results.csv` (baseline + LOO results) and `loo_table1_stability_counts.csv` (the $[k/16]$ counts annotating highlighted cells). |
| `AssembleTable2_Main.R` | Reads the two LOO CSVs and writes `../graphics/Tables/table1_v2.tex` (Main Table 2). Run *after* `RegionalAndTrendAnalysis_LOO_MainTable2.R`. |
| `TiDetectionLimitDiagnostic_Methods.R` | Diagnostic for Ti detection-limit issue; supports the Ti caveat in Main Methods. |
| `WeaningPerTuskEstimates_Supplement.R` | Supp Sec. 1.1 — per-tusk weaning posterior summaries and forest plot. |
| `PerTuskBaSrSignals_Supplement.R` | Supp Fig. `fig:per_tusk_basr_zoom` — per-tusk Ba and Sr signals, first 15 yr. |
| `RegionalDispersionAndLDA_Supplement.Rmd` | Supp Figs. `fig:boxtrace` (regional boxplots) and `fig:ldareg` (LDA projection). |
| `PearsonCorrelationHeatmap_Supplement.Rmd` | Supp Fig. `fig:elcor` (Supp Sec. 6). |
| `BoronRobustnessForest_Supplement.R` | Supp Fig. `fig:b_forest` (Supp Sec. 7) — LOO + age-cutoff sensitivity for boron. |

## Reproducing Main Table 2

```r
source("RegionalAndTrendAnalysis_LOO_MainTable2.R")   # writes the two CSVs (slow: 2 windows × 17 tusks × 10,000 perms)
source("AssembleTable2_Main.R")                       # builds graphics/Tables/table1_v2.tex from the CSVs
```

The first script implements three tests per element: East–West permutation on per-tusk raw-ppm medians, East–West permutation on per-tusk Theil-Sen slopes, and the two-stage Arctic-wide trend estimator (per-tusk Theil-Sen slopes aggregated by Wilcoxon signed-rank against zero). Each is run on the full and overlap windows, with 16 leave-one-tusk-out sensitivity iterations.

## Data files

| File | Used by |
|---|---|
| `AllSignals_Warped_ppm.csv` | `00_DataPreparation_Shared.R`, `HeavyElementEvents_Main.R`, `BoronRobustnessForest_Supplement.R` |
| `Signals_Discrete_ppm.csv` | `PCABiplotRegional_Main.R`, weaning scripts, `RegionalDispersionAndLDA_Supplement.Rmd`, `PearsonCorrelationHeatmap_Supplement.Rmd`, `ElementTrendsByYearAndAge_Main.Rmd` |
| `growth_process_all_tusks.csv` | `HeavyElementEvents_Main.R` |

NOTE: The data files (.csv) will be uploaded at acceptance of paper and by reviewer request.
