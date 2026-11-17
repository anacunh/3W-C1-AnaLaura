
# Supervised Classification of Undesirable Events in Offshore Oil Wells — 3W Dataset

## 1. Project Overview

This repository documents the computational development of the C1 research project, focused on supervised classification of undesirable events in offshore oil wells using the Petrobras 3W Dataset, version 2.0.0.

The main research question is whether deep learning models applied to raw multivariate time series outperform tree-based classifiers trained on statistical features when evaluation is performed on independent wells.

**Researcher:** Ana Laura Ribeiro da Cunha  
**Institution:** Universidade Federal do Pará (UFPA)  
**Course:** Special Topics in Artificial Intelligence Applied to Geophysical Data  
**Project:** Block C — C1: Supervised Event Classification

## 2. Dataset

The research uses the public Petrobras 3W Dataset (version 2.0.0).

Dataset characteristics:

- 2,228 time-series instances;
- 10 operational categories (normal operation and nine undesirable events);
- 1,119 real instances;
- 1,089 simulated instances;
- 20 expert-drawn instances.

Official dataset repository: https://github.com/petrobras/3W

The original dataset files are not redistributed in this repository.

## 3. Repository Structure

| Notebook | Description |
|---|---|
| `01_Exploracao_Dataset_3W.ipynb` | Initial dataset exploration, Parquet inspection, sensor identification and visualization. |
| `02_Reconhecimento_Categorias_3W.ipynb` | Operational categories, data origins, temporal labels and event inspection. |
| `03_Particionamento_Dataset_3W.ipynb` | Experimental partitioning, candidate protocols V1–V3, GroupKFold diagnostics and random-split comparison. |
| `04_Exemplo_Marins_2021_Extracao_Atributos.ipynb` | Partial methodological reproduction of statistical feature extraction from temporal windows, based on Marins et al. (2021). |
| `05_Analise_Exploratoria_V3_C1.ipynb` | Class distribution, sensor availability, missing data, descriptive statistics and correlation analysis using V3 training data. |

## 4. Initial Dataset Exploration

The initial activities included:

- Identification of operational categories and data origins;
- Inspection of real time-series instances;
- Investigation of normal, transient and event labels;
- Identification of available sensors and missing measurements;
- Analysis of class distribution across well identifiers.

## 5. Experimental Partitioning — V3

Three candidate partitioning strategies (V1, V2 and V3) were developed using real instances and well identifiers.

The current shared candidate protocol, V3, contains 1,119 real instances from 40 distinct well identifiers:

| Subset | Wells | Instances |
|---|---:|---:|
| Training | 28 | 528 |
| Validation | 6 | 183 |
| Test | 6 | 408 |

No well identifier overlaps the three subsets.

The protocol was also investigated using GroupKFold on the training partition and a random instance-level split for comparison.

The analyses identified:

- Severe class imbalance;
- Uneven class distribution across wells;
- Rare event categories concentrated in a small number of wells;
- Absence of classes 1, 3, 6 and 8 from the fixed validation subset;
- Risk of information leakage when splitting instances randomly.

V3 is maintained as the shared candidate protocol for C1 and C2. Its methodological limitations remain documented and require consideration during model development and evaluation.

## 6. Reference Methodology — Marins et al. (2021)

A partial methodological reproduction was performed using a real class-2 instance.

The activity involved extracting nine statistical features from temporal windows of the P-TPT sensor.

The resulting feature table and visualizations illustrate how time-series measurements can be transformed into statistical descriptors for traditional machine learning algorithms.

This activity does not constitute a complete reproduction of the paper or a trained classification model.

## 7. Session 05 — Exploratory Data Analysis

**Date:** November 17, 2026

Exploratory analyses were performed using the V3 real training partition, comprising 528 Parquet instances.

### Class Distribution

Classes 0 and 4 accounted for 50.76% and 27.08% of training instances, respectively, representing 77.84% combined.

### Sensor Availability and Data Quality

All 528 training files were successfully inspected, comprising 16,140,035 temporal records.

Sensors with high availability included:

- P-TPT: 464 files;
- P-PDG: 451 files;
- T-TPT: 450 files;
- P-ANULAR: 431 files.

P-PDG remained constant in 339 of the 451 files containing valid measurements.

### Descriptive Statistics

Per-file descriptive statistics were calculated for P-TPT, T-TPT and P-ANULAR, including mean, standard deviation, minimum, median and maximum.

At least one of these sensors contained valid measurements in 470 files.

### Correlation Analysis

Median within-file Pearson correlations were calculated using samples of up to 200 measurements per file.

| Sensor Pair | Median Correlation | Files |
|---|---:|---:|
| P-TPT / P-ANULAR | 0.634 | 417 |
| T-TPT / P-ANULAR | 0.523 | 421 |
| P-TPT / T-TPT | 0.170 | 435 |

These results are exploratory and do not represent classification performance.

The analyses support further investigation of sensor selection, temporal labeling and preprocessing.

## 8. Planned Modeling

The following activities are planned:

- PCA, UMAP and clustering for exploratory multivariate analysis;
- Temporal window construction and statistical feature extraction;
- Traditional machine learning baselines, including Random Forest and XGBoost;
- Deep learning models applied to multivariate time series;
- Evaluation using macro F1-score, confusion matrices and well-independent validation;
- Model interpretation using SHAP;
- Uncertainty and robustness assessment.

These modeling experiments have not yet been completed.

## 9. Execution Environment

The analyses were developed in Python using Google Colab.

Libraries used across the initial activities include:

- Pandas;
- NumPy;
- Matplotlib;
- Requests;
- Scikit-learn.

The notebooks document the procedures and should be executed with access to the official Petrobras 3W Dataset.

## 10. Project Status

**Current stage:** Exploratory analysis completed; unsupervised exploratory methods, preprocessing and supervised modeling pending.

**Completed:**

- Dataset exploration;
- Operational category inspection;
- V3 candidate partitioning;
- Partial methodological reproduction of Marins et al. (2021);
- Session 05 exploratory data analysis.

**Pending:**

- PCA, UMAP and clustering;
- Temporal window preparation;
- Model training and evaluation;
- Interpretability and uncertainty analyses;
- Scientific manuscript development.

## 11. Related Research

This C1 research project is developed in parallel with C2, coordinated by Ananda do Socorro Correa da Costa, which investigates unsupervised anomaly detection and early warning of undesirable events.

Both studies belong to Block C and share the V3 candidate partitioning protocol.

## 12. Dataset Reference

Petrobras 3W Dataset:

https://github.com/petrobras/3W

Official documentation should be consulted for event descriptions, data organization and labeling conventions.
