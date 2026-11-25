# Session 12 — Class eligibility and V4 training preparation

Date: November 25, 2026. See [notebook 12](../../12_Auditoria_Classes_e_Preparacao_V4_C1.ipynb).

This directory contains the 17 original CSV audit outputs and the original JSON configuration. All 18 Git blob hashes were matched to the uploaded archive before publication. The JSON also records the SHA-256 checksums and byte counts of the CSVs.

## Experimental scope

Only the real V4 training feature matrix was used (321,929 windows, 478 files, 27 wells). The fixed V4 validation and test subsets were not processed in this session. A minimum of three training wells per temporal class was adopted to support three-fold well-independent validation. This threshold is a project methodological choice, not a universal minimum or an instructor-prescribed threshold.

| Scenario | Eligible labels | Windows | Original matrix retained |
|---|---|---:|---:|
| A — Developed events and normal operation only | 0, 2, 4, 8 | 160,770 | 49.94% |
| B — Merge transient labels into corresponding events | 0, 2, 4, 7, 8, 9 | 304,334 | 94.53% |
| C — Distinct temporal labels (primary) | 0, 2, 4, 8, 102, 107, 108, 109 | 303,991 | 94.43% |

All scenarios retain 447 files and 27 wells. Only labels 7 and 9 gain distinct well coverage when transient labels are merged. In scenario B, 80,939 of 80,973 class-7 windows are transient; merging therefore changes the target definition substantially. Global F1 scores across different label spaces are not directly comparable.

## Fixed internal folds

30,000 candidate assignments were evaluated with seed 42, yielding 984 viable assignments for scenario C. The selected assignment has 18 training wells and 9 internal validation wells per fold, with all eligible classes present in every fold for each scenario. Selection used only class/window counts, not classifier performance. The imbalance score (0.06529776462546492) is a partition objective, not a model metric.

## Preprocessing and limitations

All 25 statistical attributes have finite observations in each internal training fold. No infinite values or inconsistent availability flags were detected. Median imputation will be fitted on training data only within each fold. Five binary sensor-availability indicators are retained in the initial feature set. Sensor availability varies markedly by label, motivating a planned ablation of the full 30 predictors, the 25 statistical features alone, and the five availability indicators alone. Removing explicit indicators may not eliminate all missingness-related information.

Scenario C excludes 17,938 windows; excluded labels remain in the original matrix and are outside the current closed-set classification scope. The fixed validation/test temporal-label coverage and excluded-window counts must be reported at evaluation. Overlapping windows and unequal well contributions remain limitations. No V4 classifier was trained and no imputation was applied in this session.

## Artifacts

- [Experimental configuration](12_Protocolo_Experimental_V4.json): authoritative settings, class lists, fold assignments, predictor order and CSV integrity checks.
- [Fold map](12_Mapa_Folds_por_Poco.csv): fixed well assignments.
- [Class eligibility](12_Protocolo_Elegibilidade_Classes.csv): inclusion/exclusion decisions and rationale.
- [Scenario summary](12_Resumo_Elegibilidade_Cenarios.csv): retained/excluded counts.
- [Fold coverage](12_Cobertura_Classes_Folds.csv): class counts by scenario and fold.
- [Imputation feasibility](12_Viabilidade_Imputacao.csv): fold-specific missingness diagnostics.
- Remaining CSVs document temporal-label support, sensor availability and detailed audits.

Use these original artifacts in Session 13 rather than recomputing or optimizing the folds. The generated feature Parquet files remain in the project working environment.
