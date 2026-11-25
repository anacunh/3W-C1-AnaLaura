# Supervised Classification of Undesirable Events in Offshore Oil Wells — 3W Dataset

## 1. Project Overview

This repository documents the computational development of the C1 research project, focused on supervised classification of undesirable events in offshore oil wells using the Petrobras 3W Dataset, version 2.0.0.

The main research question is whether deep learning models applied to raw multivariate time series outperform tree-based classifiers trained on statistical features when evaluation is performed on independent wells.

**Researcher:** Ana Laura Ribeiro da Cunha  
**Institution:** Universidade Federal do Pará (UFPA)  
**Course:** Special Topics in Artificial Intelligence Applied to Geophysical Data  
**Project:** Block C — C1: Supervised Event Classification  
**Status:** Session 12 completed; V4 class eligibility and internal folds fixed.  
**Last updated:** November 25, 2026.

## 2. Dataset

The research uses the public Petrobras 3W Dataset (version 2.0.0):

- 2,228 time-series instances;
- 10 operational categories (normal operation and nine undesirable events);
- 1,119 real instances;
- 1,089 simulated instances;
- 20 expert-drawn instances.

The selected real instances comprise 40 distinct well identifiers. Real instances constitute the primary experimental dataset. Synthetic experiments are planned as complementary sensitivity analyses, not as replacements for independent real-well evaluation.

Official dataset repository: https://github.com/petrobras/3W

The original dataset files are not redistributed in this repository.

## 3. Repository Structure

| Notebook | Description |
|---|---|
| [01 — Dataset exploration](01_Exploracao_Dataset_3W.ipynb) | Initial exploration, Parquet inspection, sensor identification and visualization. |
| [02 — Operational categories](02_Reconhecimento_Categorias_3W.ipynb) | Operational categories, data origins, temporal labels and event inspection. |
| [03 — Experimental partitioning](03_Particionamento_Dataset_3W.ipynb) | Candidate protocols V1–V3, GroupKFold diagnostics and random-split comparison. |
| [04 — Marins et al. (2021) example](04_Exemplo_Marins_2021_Extracao_Atributos%20%281%29.ipynb) | Partial methodological reproduction of statistical feature extraction. |
| [05 — V3 exploratory analysis](05_Analise_Exploratoria_V3_C1.ipynb) | Class distribution, sensor availability, missing data, descriptive statistics and correlations. |
| [06 — Unsupervised exploration](06_Aprendizado_Nao_Supervisionado_C1.ipynb) | PCA, UMAP and K-means using V3 training file-level features. |
| [07 — Temporal windows](07_Janelas_Temporais_C1.ipynb) | Pilot window construction, temporal-label consistency and statistical features. |
| [08 — Initial supervised baseline](08_Preparacao_Treinamento_e_Baseline_C1.ipynb) | V3 feature matrix and preliminary Random Forest experiment. |
| [09 — Preprocessing diagnosis](09_Diagnostico_Preprocessamento_C1.ipynb) | Sensor-related exclusions, class coverage and grouped validation diagnostics. |
| [10 — V4 preprocessing audit](10_Preprocessamento_Final_C1.ipynb) | Verification of the adjusted V4 protocol and full training-file audit of sensor eligibility rules. |
| [11 — V4 feature matrix](11_Construcao_Matriz_Atributos_V4_C1.ipynb) | Construction and integrity verification of the V4 training feature matrix. |
| [12 — Class eligibility and V4 training preparation](12_Auditoria_Classes_e_Preparacao_V4_C1.ipynb) | Temporal-label audit, eligibility criteria, fixed internal folds and missing-feature diagnostics. |

Additional files:

- [`protocolo_v3/`](protocolo_v3/): historical V3 partitioning artifacts.
- [`protocolo_v4/`](protocolo_v4/): verified original V4 well and instance manifests, Session 10 audit CSV and protocol documentation.
- [`protocolo_v4/sessao_12/`](protocolo_v4/sessao_12/): original Session 12 audit CSVs, experimental configuration and documentation.
- [`requirements.txt`](requirements.txt): dependencies reviewed through Session 11, including joblib; versions are not yet pinned.
- `README.md`: project documentation.

The original V4 manifests and Session 10 audit CSV are published in [`protocolo_v4/`](protocolo_v4/). Their Git blob hashes were verified against the original files. Generated Parquet feature files remain in the project working environment.

## 4. Initial Dataset Exploration and Reference Methodology (Sessions 01–05)

The initial activities included identification of operational categories and data origins, inspection of real time series, investigation of normal, transient and developed-event labels, sensor availability and missing measurements, and class distribution across wells.

A partial methodological reproduction based on Marins et al. (2021) extracted nine statistical features from temporal windows of the P-TPT sensor in a real class-2 instance. This was a methodological example, not a complete reproduction of the paper or a trained classification model.

### Session 05 — Exploratory Data Analysis (V3)

**Date:** November 17, 2026.

All 528 V3 training files were inspected, comprising 16,140,035 temporal records. File categories 0 and 4 accounted for 50.76% and 27.08% of training instances, respectively (77.84% combined).

Sensor availability:

| Sensor | Files with valid measurements |
|---|---:|
| P-TPT | 464 |
| P-PDG | 451 |
| T-TPT | 450 |
| P-ANULAR | 431 |

P-PDG remained constant in 339 of the 451 files containing valid measurements. Per-file descriptive statistics (mean, standard deviation, minimum, median and maximum) were calculated for P-TPT, T-TPT and P-ANULAR. At least one of these sensors contained valid measurements in 470 files.

Median within-file Pearson correlations were calculated using samples of up to 200 measurements per file:

| Sensor pair | Median correlation | Files |
|---|---:|---:|
| P-TPT / P-ANULAR | 0.634 | 417 |
| T-TPT / P-ANULAR | 0.523 | 421 |
| P-TPT / T-TPT | 0.170 | 435 |

These exploratory results do not represent classification performance.

## 5. Experimental Partitioning — V3 and Adjusted V4

Three initial candidate partitioning strategies (V1–V3) were developed using real instances and well identifiers. V3 is retained as a historical protocol for the initial experiments. The adjusted V4 protocol is the current shared partition for C1 and C2.

| Subset | V3 wells | V3 instances | V4 wells | V4 instances |
|---|---:|---:|---:|---:|
| Training | 28 | 528 | 28 | 535 |
| Validation | 6 | 183 | 6 | 176 |
| Test | 6 | 408 | 6 | 408 |
| **Total** | **40** | **1,119** | **40** | **1,119** |

V4 was adjusted to improve category representation in the fixed validation subset while preserving the original six test wells. WELL-00030 moved from training to validation and WELL-00022 moved from validation to training. No well identifier overlaps the training, validation and test subsets. The partition is not to be optimized using classifier scores.

V3 validation lacked file categories 1, 3, 6 and 8. V4 validation contains seven of the ten file categories, with categories 1, 3 and 6 absent. This is a file-level diagnostic; temporal-label coverage must be assessed separately.

The fixed V4 test subset is reserved for final evaluation. Internal model selection and preprocessing must use training data, with well-independent validation and no leakage across groups.

## 6. Exploratory Modeling and Initial Baseline (Sessions 06–09)

### Session 06 — Unsupervised exploration

PCA, UMAP and K-means were applied to six file-level statistical features from 470 V3 training instances. The first two principal components explained 63.81% of the variance. K-means with two clusters achieved a silhouette score of 0.702, but its adjusted Rand index against file categories was only 0.0847. Thus, cluster separation did not imply reliable recovery of operational categories.

### Session 07 — Temporal-window pilot

A pilot instance was segmented into windows of 100 samples with a stride of 50 samples. The experiment retained 177 valid windows and identified 76 rejected windows due to missing or changing temporal labels. Constant signals produced undefined skewness and kurtosis, motivating further feature-quality assessment.

### Session 08 — Initial supervised baseline (V3)

The initial feature extraction produced 162,326 windows with 27 statistical attributes, using a strict requirement for three complete sensors and an exploratory mapping of transient labels to their corresponding event classes.

Only 15 of the 28 V3 training wells contributed valid windows. A preliminary Random Forest experiment used an internal split of 12 training wells and three validation wells, with only classes 0, 2 and 9 represented in validation.

- Macro F1 over the classes present in internal validation: **0.5897**.
- Class 9 F1: **0.000**.
- Seven classes were absent from this internal validation subset.

These are preliminary V3 results, not performance on the fixed V4 validation or test sets.

### Session 09 — Preprocessing and generalization diagnosis

The strict three-sensor rule excluded 13 V3 training wells. Several event classes were represented by only one well.

A grouped validation diagnostic on 13,071 sampled windows from 15 wells yielded a macro F1 of **0.2226** for classes represented in multiple wells (0, 2, 4, 8 and 9). The three fold-level macro F1 scores over classes present in each fold were 0.1766, 0.2373 and 0.0512.

These diagnostics use different samples and evaluation definitions from Session 08 and should not be interpreted as a direct controlled comparison. They reveal important limitations in cross-well generalization and motivate the revised preprocessing strategy.

## 7. Session 10 — V4 Sensor Availability Audit

All 535 V4 training files were audited successfully. The audit is a distinct methodological stage, performed before definitive model training.

Five sensors were investigated:

- P-TPT
- T-TPT
- P-ANULAR
- P-JUS-CKGL
- P-MON-CKP

Three eligibility rules were compared using the same V4 training partition:

| Sensor eligibility rule | Files with valid windows | Wells represented | Valid windows |
|---|---:|---:|---:|
| All three original sensors complete | 434 | 15 | 220,625 |
| At least two of five sensors complete | 478 | 27 | 321,929 |
| At least one of five sensors complete | 535 | 28 | 342,004 |

The two-sensor criterion was selected as the working candidate for feature extraction. It retains 94.13% of the windows available under the one-sensor rule and increases well coverage relative to the original three-sensor requirement.

The one-sensor alternative remains available for a sensitivity experiment. Under the two-sensor criterion, WELL-00008 contributes no windows. This is an explicit coverage limitation, not evidence of improved classifier performance.

## 8. Session 11 — V4 Training Feature Matrix

### Experimental configuration

| Parameter | Configuration |
|---|---|
| Dataset subset | V4 real training instances only |
| Window length | 100 samples |
| Stride | 50 samples (50% overlap) |
| Nominal sampling interval | 1 second |
| Sensor requirement | At least two of the five selected sensors complete |
| Temporal labels | Original homogeneous labels retained |
| Statistical features per sensor | Mean, standard deviation, slope, 10th and 90th percentiles |
| Missing sensor features | NaN; not yet imputed |
| Sensor availability | Five binary indicators |

The resulting schema contains 37 columns: 25 statistical features, five sensor availability indicators and seven metadata fields. Metadata and labels are not to be used as unintended predictors.

### Integrity verification

| Metric | Result |
|---|---:|
| Training files processed | 535 |
| Files with valid windows | 478 |
| Files without valid windows | 57 |
| Valid windows extracted and verified | **321,929** |
| Wells contributing valid windows | **27** |

The feature files were saved in Parquet format in the project working environment. Validation and test feature extraction was not part of Session 11.

### Original temporal-label distribution

| Temporal phase | Windows | Percentage |
|---|---:|---:|
| Normal operation (0) | 138,404 | 42.99% |
| Transient phases (101–109) | 149,475 | 46.43% |
| Developed events (1–9) | 34,050 | 10.58% |
| **Total** | **321,929** | **100.00%** |

| Original label | Windows | Wells |
|---|---:|---:|
| 0 | 138,404 | 27 |
| 1 | 58 | 1 |
| 2 | 299 | 5 |
| 3 | 10,967 | 1 |
| 4 | 20,211 | 3 |
| 5 | 68 | 1 |
| 6 | 248 | 1 |
| 7 | 34 | 1 |
| 8 | 1,856 | 7 |
| 9 | 309 | 2 |
| 101 | 130 | 1 |
| 102 | 1,401 | 5 |
| 103 | 0 | 0 |
| 104 | 0 | 0 |
| 105 | 6,037 | 1 |
| 106 | 87 | 1 |
| 107 | 80,939 | 3 |
| 108 | 57,124 | 7 |
| 109 | 3,757 | 6 |

The distinction between file categories and temporal labels is essential. For example, transient label 107 has 80,939 windows across three wells, whereas developed-event label 7 has only 34 windows from one well.

Developed-event labels 1, 3, 5, 6 and 7 each occur in only one training well. In addition, WELL-00002 and WELL-00022 contribute 89,738 and 61,456 windows, respectively (approximately 46.97% combined). Overlapping windows are not independent observations.

These findings motivate a formal audit of class eligibility, transient-label treatment, group-aware validation and training imbalance. At the end of Session 11, no class exclusions, label mapping, imputation or V4 classifier performance had been established. Session 12 subsequently defined the experimental class scope and fold assignments.

## 9. Methodological Guidance — Checkpoint 1 (November 18, 2026)

The following directions were discussed with the course instructor:

- Continue with the 3W dataset as the primary dataset; the selected population of 40 wells was confirmed.
- Retain the data audit as a distinct methodological stage and use its findings to inform balancing and representativeness.
- Exclude insufficiently supported event categories where scientifically justified, explicitly reporting the criteria, excluded classes and implications for the scope of evaluation.
- Use well-independent folds and document limitations arising from sparse class coverage.
- Use synthetic experiments for complementary sensitivity analysis, with parameters grounded in real observations; distinguish synthetic and real-data results.
- Report unsuccessful experiments and limitations alongside positive findings.
- Consider an additional comparable dataset for external evaluation if one becomes available, without delaying the primary study.

The instructor's guidance did not establish a specific numerical exclusion threshold or a final list of excluded classes. The project adopted a three-well eligibility criterion in Session 12, before V4 model training.

## 10. Session 12 — Class Eligibility and Training Preparation

The original 321,929-window training matrix was audited by temporal label, file and well. Classes 1, 3, 5, 6 and 7 (developed events) occur in only one well each; class 9 occurs in two wells, with 98.71% of its windows from one well. Class eligibility was defined as representation in at least three training wells, to support three-fold internal validation.

| Scenario | Eligible labels | Retained windows | Original matrix retained |
|---|---|---:|---:|
| A — Exclude transients | 0, 2, 4, 8 | 160,770 | 49.94% |
| B — Merge transients with events | 0, 2, 4, 7, 8, 9 | 304,334 | 94.53% |
| C — Distinct labels (primary) | 0, 2, 4, 8, 102, 107, 108, 109 | 303,991 | 94.43% |

All scenarios retain 447 files and 27 wells. Only classes 7 and 9 gain well coverage by merging transients. In scenario B, 99.96% of class-7 windows are transient. Different label spaces must not be treated as directly comparable classification tasks.

A fixed well assignment was selected from 30,000 candidate partitions (seed 42; 984 viable for scenario C), using class/window counts only. Each of the three internal folds has 18 training wells and nine validation wells, with all eligible labels present in each fold for all three scenarios. The fixed V4 validation and test subsets remain separate and unprocessed.

All 25 statistical attributes have finite observations in every internal training fold. No infinite values or inconsistent availability flags were detected. Median imputation will be fitted only on the training portion of each fold. Sensor availability differs substantially across classes, motivating planned ablations of the 30 full predictors, 25 statistical features alone, and five availability flags alone.

The primary scenario is a restricted closed-set task, not recognition of every 3W event. The original matrix remains intact. No V4 classifier has yet been trained, and no imputation has been applied. See [Session 12 artifacts and configuration](protocolo_v4/sessao_12/).

## 11. Planned Modeling and Evaluation

The next stages include:

- Reuse the fixed Session 12 class definitions and internal folds; audit temporal-label coverage of the fixed V4 validation/test subsets at their designated evaluation stages.
- Preserve the fixed V4 partition and report excluded labels and windows.
- Fit imputation, scaling where appropriate, and any balancing procedures on training data only, within each fold when applicable.
- Train reproducible statistical-feature baselines, including Random Forest and XGBoost; compare unweighted and class-weighted training and investigate sensor-availability ablation.
- Investigate deep learning on raw multivariate time series under a comparable evaluation protocol.
- Evaluate macro F1, per-class metrics, confusion matrices and well-independent generalization; explicitly report class coverage and evaluation scope.
- Investigate model interpretation (e.g., SHAP), uncertainty and robustness.
- Conduct complementary synthetic sensitivity experiments and, if feasible, external-data evaluation.
- Develop comparative figures and document both successful and unsuccessful experiments.

## 12. Execution Environment and Reproducibility

The analyses were developed in Python using Google Colab. Libraries used include pandas, NumPy, Matplotlib, requests, scikit-learn, umap-learn and Parquet support through PyArrow.

The notebooks document the computational procedures and require access to the official Petrobras 3W Dataset and relevant project artifacts. Some notebook cells use Google Drive paths or interactive uploads; these paths must be adapted to the execution environment.

The V4 manifests and Session 10 audit have been published and verified. The dependency list has been reviewed through Session 11; exact package versions, environment verification and adaptation of notebook paths remain necessary before final reproducibility can be claimed.

## 13. Project Status and Scientific Writing

**Current stage:** Sessions 01–12 completed. The V4 training feature matrix, class eligibility, fixed internal folds and preprocessing plan are documented; V4 supervised modeling is next.

| Milestone | Status |
|---|---|
| Dataset exploration and historical V3 partitioning | Completed |
| File-level exploratory and unsupervised analyses | Completed |
| Initial V3 Random Forest and grouped diagnostic | Completed (preliminary) |
| Adjusted V4 partition and sensor audit | Completed |
| V4 training feature extraction | Completed |
| Class eligibility, transient strategy and grouped folds | Completed |
| Session 12 audit artifacts and experimental configuration | Published and verified |
| Definitive V4 baselines and deep learning comparison | Pending |
| Interpretability, uncertainty and synthetic sensitivity | Pending |
| V4 manifest and audit publication; dependency review through Session 11 | Completed |
| Exact environment versions and end-to-end reproduction | Pending |
| Scientific manuscript | Drafting deferred until computational workflow is complete |
| Comparative model figures | Pending |

Upcoming course milestones:

- December 2, 2026: AI Checkpoint 2 — initial reproducible results and first comparative figure.
- December 3, 2026: Scientific Writing R03 — first draft of the introduction and data section.
- December 14, 2026: AI final deliverables.
- December 16–17, 2026: AI final presentations.
- December 17, 2026: Scientific Writing final manuscript V1 and accompanying materials.

Technical session records and repository documentation continue throughout the computational workflow. Manuscript drafting is deferred until the computational work is complete. The course milestones listed above remain unchanged unless the instructor revises them. Reported results must distinguish historical V3 diagnostics, V4 data-preparation results and future V4 classifier performance.

## 14. Related Research

C1 is developed in parallel with C2, coordinated by Ananda do Socorro Correa da Costa, which investigates unsupervised anomaly detection and early warning of undesirable events.

Both studies belong to Block C and share the adjusted V4 partitioning protocol. Their distinct learning objectives and evaluation scopes must be considered when comparing results.

## 15. Dataset Reference

Petrobras 3W Dataset: https://github.com/petrobras/3W

Official documentation should be consulted for event descriptions, data organization and labeling conventions.
