# Dataset Audit

## Audit Purpose

This audit records the verified structure and initial data-quality findings for the dataset before any preprocessing or model training begins. Its purpose is to separate observed facts from future cleaning decisions, so that subsequent exploratory data analysis (EDA), preprocessing, and modelling work can be reproducible and explicitly justified.

## Dataset Under Audit

| Item | Verified value |
| --- | --- |
| Dataset name | Heart Failure Prediction Dataset |
| Local path | `ml/data/raw/heart.csv` |
| Shape | `(918, 12)` |
| Observations | 918 |
| Input features | 11 |
| Target | `HeartDisease` |
| Target mapping | `0` = Normal; `1` = Heart disease |

The raw dataset is intentionally excluded from Git tracking.

## Audit Methodology

The completed audit checks cover structure, data types, missing values, duplicate rows, categorical consistency, suspicious zero values, negative values, unique values, target distribution, IQR-based outlier screening, and initial feature-target relationships.

These checks are exploratory. A flagged value is not automatically erroneous, and the findings do not constitute final preprocessing decisions.

## 1. Basic Structure

The dataset contains 918 observations and 12 columns: 11 input features and the `HeartDisease` target variable.

## 2. Data Types

| Column | Verified data type |
| --- | --- |
| `Age` | `int64` |
| `Sex` | `str` |
| `ChestPainType` | `str` |
| `RestingBP` | `int64` |
| `Cholesterol` | `int64` |
| `FastingBS` | `int64` |
| `RestingECG` | `str` |
| `MaxHR` | `int64` |
| `ExerciseAngina` | `str` |
| `Oldpeak` | `float64` |
| `ST_Slope` | `str` |
| `HeartDisease` | `int64` |

## 3. Missing Values

`df.isnull().sum()` returned zero for every column, with zero total missing/NaN cells.

| Check | Verified result |
| --- | --- |
| Missing/NaN cells | 0 |
| Columns with reported NaN values | None |

This result applies to NaN values only. Numeric zeros can still be suspicious or semantically missing in context; therefore, the dataset should not be described simply as having no missing information.

## 4. Duplicate Records

The duplicate row count is **0**.

## 5. Categorical Value Analysis

The following expected category sets were verified.

| Feature | Verified categories | Explicitly verified observed counts |
| --- | --- | --- |
| `Sex` | `M`, `F` | `M` = 725; `F` = 193 |
| `ChestPainType` | `TA`, `ATA`, `NAP`, `ASY` | `ASY` = 496; `NAP` = 203 |
| `FastingBS` | `0`, `1` | Not yet documented |
| `RestingECG` | `Normal`, `ST`, `LVH` | `Normal` = 552 |
| `ExerciseAngina` | `Y`, `N` | `N` = 547 |
| `ST_Slope` | `Up`, `Flat`, `Down` | `Flat` = 460 |

Only the category counts shown above have been verified and documented at this stage.

## 6. Numerical Feature Analysis

The numerical features considered in the initial review were `Age`, `RestingBP`, `Cholesterol`, `FastingBS`, `MaxHR`, and `Oldpeak`.

| Feature or group | Verified high-level observation |
| --- | --- |
| `Age` | Mean is approximately 53. |
| `RestingBP` | Concentrated approximately around 120–140 for many observations. |
| `MaxHR` | Commonly falls approximately in the 120–160 range. |
| `Cholesterol` | Distribution is strongly affected by zero-valued observations. |

These are approximate distribution observations, not exact minimum or maximum ranges.

## 7. Suspicious Zero Values

| Feature | Zero Count | Audit Interpretation |
| --- | --- | --- |
| `RestingBP` | 1 | Invalid/suspicious value requiring a preprocessing decision. |
| `Cholesterol` | 172 | Suspicious; may represent missing or unrecorded data and requires future preprocessing consideration. |
| `Oldpeak` | 368 | Not automatically an error; it should not be removed solely because it is zero. |

No cleaning or imputation has been performed. Suspicious zeros are audit findings, not final preprocessing decisions.

## 8. Invalid / Negative Value Analysis

| Feature | Negative value count |
| --- | --- |
| `Age` | 0 |
| `RestingBP` | 0 |
| `Cholesterol` | 0 |
| `MaxHR` | 0 |
| `Oldpeak` | 13 |

Negative `Oldpeak` values were found. They should not automatically be treated as data-entry errors: depending on the feature definition and clinical context, they may represent ST-segment depression/elevation-related measurements. They require contextual consideration and further EDA, and remain available for further analysis unless later evidence justifies another decision.

## 9. Specific Data Quality Findings

### 9.1 Cholesterol = 0

There are 172 observations with `Cholesterol = 0`. This is treated as a suspicious, likely missing-data representation for future preprocessing consideration. No imputation has been performed, and no mean or median strategy has been selected.

### 9.2 RestingBP = 0

One suspicious/invalid observation was identified. It has **not** been deleted and requires a preprocessing decision.

| Field | Verified value |
| --- | --- |
| Original dataframe index | 449 |
| `Age` | 55 |
| `Sex` | `M` |
| `ChestPainType` | `NAP` |
| `Cholesterol` | 0 |
| `RestingBP` | 0 |
| `HeartDisease` | 1 |

### 9.3 Negative Oldpeak Values

Thirteen negative `Oldpeak` values were found. They require contextual analysis during EDA rather than automatic removal. Their validity has not yet been formally determined.

## 10. Target Distribution

| Target Value | Meaning | Count | Percentage |
| --- | --- | --- | --- |
| `0` | Normal | 410 | 44.66% |
| `1` | Heart disease | 508 | 55.34% |

This is a moderately imbalanced binary target distribution. The distribution should be considered during model evaluation; no evaluation approach has been selected yet.

## 11. Unique Value Analysis

Verified examples from `df.nunique()` are:

| Feature | Unique values |
| --- | --- |
| `Sex` | 2 |
| `ChestPainType` | 4 |
| `Age` | 50 |
| `Cholesterol` | 222 |

Unique-value counts for the remaining columns are not yet documented here.

## 12. Outlier Screening

An initial IQR-based screening reported the following flagged observations.

| Feature | IQR-flagged observations |
| --- | --- |
| `Cholesterol` | 183 |
| `FastingBS` | 214 |
| `RestingBP` | 28 |
| `Oldpeak` | 16 |
| `MaxHR` | 2 |

IQR flags are a screening mechanism, not proof that a record is invalid. `FastingBS` is binary, so conventional IQR counts do not carry the same meaning as for continuous features. The `Cholesterol` count is strongly affected by zero-valued observations; `RestingBP` flags may include unusually high values and the suspicious zero value; and `Oldpeak` flags require domain and contextual interpretation.

## 13. Initial Feature-Target Relationships

The initial correlation analysis reported these approximate associations with `HeartDisease`.

| Feature | Approximate association with `HeartDisease` |
| --- | --- |
| `Oldpeak` | +0.40 |
| `Age` | +0.28 |
| `MaxHR` | -0.40 |

Observed categorical patterns include:

- `ChestPainType = ASY` is associated with a higher observed `HeartDisease` occurrence than the other chest-pain categories.
- `ExerciseAngina = Y` is associated with a higher observed `HeartDisease` occurrence than `ExerciseAngina = N`.

**Correlation and association do not imply causation.** These are observational associations within this dataset; statistical significance has not been tested.

## 14. Data Quality Summary

| Area | Status | Summary |
| --- | --- | --- |
| Structural integrity | Verified | 918 observations, 12 columns, 11 input features, and 1 target. |
| Missing/NaN values | Verified | No NaN cells were found. |
| Duplicate records | Verified | No duplicate rows were found. |
| Categorical consistency | Verified | Expected category sets were checked. |
| Suspicious zeros | Attention required | `Cholesterol = 0` and `RestingBP = 0` require future decisions. |
| Negative values | Further investigation required | Negative `Oldpeak` values require contextual analysis. |
| Target balance | Verified | 410 Normal observations and 508 heart-disease observations. |
| Outlier screening | Further investigation required | IQR flags require statistical and contextual interpretation. |

## 15. Audit Conclusions

The dataset has 918 observations and 12 columns. No NaN values and no duplicate rows were found. Several suspicious numeric values require preprocessing decisions: `Cholesterol = 0` is the most significant issue by count, and `RestingBP = 0` is a specific invalid/suspicious observation. Negative `Oldpeak` values require contextual analysis rather than automatic removal.

Initial outlier screening identified several statistical outliers, but they require interpretation and are not automatically invalid records. The target distribution is 410 Normal observations versus 508 heart-disease observations. This audit is informative but does not represent the final preprocessing stage.

## 16. Next Steps

Planned work, not yet implemented:

1. Perform structured EDA with visualizations.
2. Investigate suspicious zero values in more detail.
3. Decide how to handle `Cholesterol = 0`.
4. Decide how to handle the `RestingBP = 0` observation.
5. Investigate negative `Oldpeak` observations during EDA.
6. Reassess outliers using statistical and domain/contextual reasoning.
7. Document final cleaning and preprocessing decisions separately.
8. Build the preprocessing pipeline only after the audit and EDA are complete.

## Reproducibility Notes

The audit was performed against:

```text
ml/data/raw/heart.csv
```

The raw dataset remains excluded from Git tracking.
