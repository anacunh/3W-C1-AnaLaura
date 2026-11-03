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

The second candidate partition (V2) contains:

- Training: 28 real wells;
- Validation: 6 real wells;
- Test: 6 real wells.

These partitions remain **provisional** and require methodological review before final adoption.

Limitations include underrepresented event categories, uneven class distribution across wells, and missing categories in the validation subset.

## 6. Planned Modeling

The supervised classification study is expected to investigate:

- Statistical feature extraction from time-series windows;
- Traditional Machine Learning classifiers, including Random Forest and XGBoost;
- Deep Learning models, including CNN 1D, LSTM, and InceptionTime;
- Evaluation using macro F1-score, confusion matrices, and well-independent testing;
- Model interpretability and uncertainty analysis.

These modeling stages are **planned and have not yet been completed**.

## 7. Execution Environment

The notebooks were developed in Python using Google Colab.

Main libraries used in the initial analyses include:

- Pandas
- NumPy
- Matplotlib
- Requests
- Scikit-learn

To reproduce the exploratory analyses, open the notebooks in Google Colab and execute their cells in order, with internet access to the official Petrobras dataset repository.

The notebooks correspond to successive stages of the project. Some experimental settings remain provisional.

## 8. Project Status

**Current stage:** dataset exploration and preliminary experimental design.

**Completed:** dataset inventory, exploratory analysis, event-category inspection, and candidate data partitioning.

**Pending:** final experimental protocol, preprocessing, model training, validation, interpretation, and final scientific reporting.

## 9. Related Research

This C1 project is developed in parallel with C2, coordinated by Ananda do Socorro Correa da Costa, which investigates unsupervised anomaly detection.

Both studies belong to Block C and will contribute to a comparative scientific manuscript.

## 10. Dataset Reference

Petrobras 3W Dataset: https://github.com/petrobras/3W

The dataset and its documentation should be consulted for the official event descriptions, data organization, and labeling conventions.
