# Supervised Classification of Undesirable Events in Offshore Oil Wells — 3W Dataset

## 1. Project Overview

This repository contains the initial computational development of the C1 project, focused on supervised classification of undesirable events in offshore oil wells using the Petrobras 3W Dataset, version 2.0.0.

The research aims to compare traditional Machine Learning models based on statistical features with Deep Learning approaches applied to multivariate time series.

**Researcher:** Ana Laura Ribeiro da Cunha  
**Course:** Special Topics in Artificial Intelligence Applied to Geophysical Data  
**Institution:** Universidade Federal do Pará (UFPA)  
**Project:** Block C — C1: Supervised Event Classification

## 2. Dataset

The research uses the public Petrobras 3W Dataset (version 2.0.0).

Dataset characteristics:

- 2,228 time-series instances;
- 10 operational categories (normal operation and nine undesirable events);
- 1,119 real instances;
- 1,089 simulated instances;
- 20 expert-drawn instances.

Official repository: https://github.com/petrobras/3W

The original dataset files are not redistributed in this repository.

## 3. Repository Structure

| Notebook | Description |
|---|---|
| `01_Exploracao_Dataset_3W.ipynb` | Initial dataset access, Parquet inspection, missing values, descriptive statistics, and sensor visualization. |
| `02_Reconhecimento_Categorias_3W.ipynb` | Event categories, instance distribution, data origins, and temporal analysis of an undesirable event. |
| `03_Particionamento_Dataset_3W.ipynb` | Preliminary experimental design, well identification, class distribution, and candidate train/validation/test partitions. |

## 4. Initial Data Exploration

The initial analyses include:

- Identification of operational event categories;
- Distribution of instances across data origins;
- Inspection of sensor availability and missing values;
- Descriptive statistics and time-series visualization;
- Investigation of normal, transient, and undesirable-event labels;
- Distribution of operational categories across real wells.

## 5. Preliminary Experimental Design

Candidate data partitions were developed using well identifiers to prevent the same real well from appearing in different training, validation, and test sets.

The current shared candidate partition (V3) contains **1,119 real instances from 40 well identifiers**:

- Training: 28 real wells, 528 file instances;
- Validation: 6 real wells, 183 file instances;
- Test: 6 real wells, 408 file instances.

No well identifier overlaps the three partitions. Classes 1, 3, 6 and 8 are missing from the fixed validation partition. The V3 protocol is a shared experimental candidate and has not received final methodological approval.

These partitions remain **provisional** and require methodological review before final adoption.

Limitations include underrepresented event categories, uneven class distribution across wells, and missing categories in the validation subset.

## 6. Session 05 — Exploratory Analysis (2026-11-17)

The session's initial analyses were completed on the V3 real training partition (528 Parquet instances):

- File-level class distribution: classes 0 and 4 account for 50.76% and 27.08% of training instances, respectively (77.84% combined).
- Full sensor-availability audit: 528/528 files read successfully; 16,140,035 time records inventoried. Class-label columns `class` and `state` are not used as predictors.
- Sensors with valid measurements in most files: `P-TPT` (464), `P-PDG` (451), `T-TPT` (450), and `P-ANULAR` (431). `P-PDG` is constant in 339 of its 451 available files.
- Per-file descriptive statistics for `P-TPT`, `T-TPT`, and `P-ANULAR`; 470 files had measurements for at least one of these three sensors.
- Median within-file Pearson correlations based on up to 200 sampled measurements per file: `P-TPT` vs `P-ANULAR` 0.634 (417 files), `T-TPT` vs `P-ANULAR` 0.523 (421 files), `P-TPT` vs `T-TPT` 0.170 (435 files).

These are **exploratory findings**, not classification performance. Sensor choice, temporal-state labeling and windowing remain provisional. The Colab notebook `05_Analise_Exploratoria_V3_C1.ipynb` is being prepared for repository upload; the complete sensor and correlation CSV outputs have not been added here.

## 7. Planned Modeling

The supervised classification study is expected to investigate:

- Statistical feature extraction from time-series windows;
- Traditional Machine Learning classifiers, including Random Forest and XGBoost;
- Deep Learning models, including CNN 1D, LSTM, and InceptionTime;
- Evaluation using macro F1-score, confusion matrices, and well-independent testing;
- Model interpretability and uncertainty analysis.

These modeling stages are **planned and have not yet been completed**.

## 8. Execution Environment

The notebooks were developed in Python using Google Colab.

Main libraries used in the initial analyses include:

- Pandas
- NumPy
- Matplotlib
- Requests
- Scikit-learn

To reproduce the exploratory analyses, open the notebooks in Google Colab and execute their cells in order, with internet access to the official Petrobras dataset repository.

The notebooks correspond to successive stages of the project. Some experimental settings remain provisional.

## 9. Project Status

**Current stage:** V3 training-set exploratory analysis completed; preparation of temporal windows and model baselines pending.

**Completed:** dataset inventory, event-category inspection, candidate V3 partitioning, and Session 05 sensor, class-distribution, descriptive-statistic and correlation analyses.

**Pending:** final experimental protocol, preprocessing, model training, validation, interpretation, and final scientific reporting.

## 10. Related Research

This C1 project is developed in parallel with C2, coordinated by Ananda do Socorro Correa da Costa, which investigates unsupervised anomaly detection.

Both studies belong to Block C and will contribute to a comparative scientific manuscript.

## 11. Dataset Reference

Petrobras 3W Dataset: https://github.com/petrobras/3W

The dataset and its documentation should be consulted for the official event descriptions, data organization, and labeling conventions.
