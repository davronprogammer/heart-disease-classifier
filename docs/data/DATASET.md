# Dataset

## Overview

This project uses Kaggle's **Heart Failure Prediction Dataset** for binary heart disease classification. Although the source dataset's title refers to heart failure prediction, this project's target column is `HeartDisease`; the task is therefore heart disease classification, not a clinical heart-failure prediction system.

## Source

- **Dataset name:** Heart Failure Prediction Dataset
- **Kaggle source:** fedesoriano / Heart Failure Prediction Dataset
- **Kaggle URL:** <https://www.kaggle.com/datasets/fedesoriano/heart-failure-prediction>
- **Original file name:** `heart.csv`

## Dataset Structure

The published dataset contains 918 observations, 11 input features, and 1 target variable, for 12 columns in total.

| Column | Role | Data type |
| --- | --- | --- |
| `Age` | Feature | Numerical |
| `Sex` | Feature | Categorical |
| `ChestPainType` | Feature | Categorical |
| `RestingBP` | Feature | Numerical |
| `Cholesterol` | Feature | Numerical |
| `FastingBS` | Feature | Categorical (binary) |
| `RestingECG` | Feature | Categorical |
| `MaxHR` | Feature | Numerical |
| `ExerciseAngina` | Feature | Categorical |
| `Oldpeak` | Feature | Numerical |
| `ST_Slope` | Feature | Categorical |
| `HeartDisease` | Target | Categorical (binary) |

## Target Variable

`HeartDisease` is the binary target variable:

- `0` = Normal
- `1` = Heart disease

Model outputs are not medical diagnoses and must not be used as a substitute for clinical assessment.

## Feature Description

| Feature | Type | Description |
| --- | --- | --- |
| `Age` | Numerical | Age of the patient, in years. |
| `Sex` | Categorical | Sex: `M` = male; `F` = female. |
| `ChestPainType` | Categorical | Chest-pain type: `TA` = typical angina; `ATA` = atypical angina; `NAP` = non-anginal pain; `ASY` = asymptomatic. |
| `RestingBP` | Numerical | Resting blood pressure, in mm Hg. |
| `Cholesterol` | Numerical | Serum cholesterol, in mm/dl. |
| `FastingBS` | Categorical (binary) | Fasting blood sugar indicator: `1` when fasting blood sugar is greater than 120 mg/dl; `0` otherwise. |
| `RestingECG` | Categorical | Resting electrocardiogram result: `Normal`; `ST` = ST-T wave abnormality; `LVH` = probable or definite left ventricular hypertrophy by Estes' criteria. |
| `MaxHR` | Numerical | Maximum heart rate achieved. |
| `ExerciseAngina` | Categorical | Exercise-induced angina: `Y` = yes; `N` = no. |
| `Oldpeak` | Numerical | ST depression measurement. |
| `ST_Slope` | Categorical | Slope of the peak-exercise ST segment: `Up` = upsloping; `Flat` = flat; `Down` = downsloping. |

## Data Storage

The raw dataset is stored locally at:

```text
ml/data/raw/heart.csv
```

The raw dataset is intentionally excluded from Git tracking through `.gitignore`. Its contents are not included in this repository documentation.

## Reproducibility

Developers should obtain the dataset from the original Kaggle source and place `heart.csv` at:

```text
ml/data/raw/heart.csv
```

## Data Quality Notes

The local file has been verified to contain 918 data rows (919 lines including the header) and to be 35,921 bytes. A formal data-quality audit has **not** yet been completed.

Missing values, duplicate records, class balance, valid ranges, and outliers will be assessed during the upcoming data audit.

## Provenance / Attribution

The Kaggle dataset states that it combines five previously separate heart-disease datasets—Cleveland, Hungarian, Switzerland, Long Beach VA, and Stalog—using 11 common features. It reports 1,190 combined observations before removal of 272 duplicates, resulting in the published 918-observation dataset. Credit is given to the original Kaggle dataset and its stated source datasets.
