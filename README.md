````markdown
# Suspicious Bank Transaction Detection Project

This project focuses on detecting suspicious bank transactions by combining:

- data cleaning and preprocessing
- exploratory data analysis (EDA)
- statistical anomaly flags, such as z-score based detection
- unsupervised anomaly detection using Isolation Forest
- supervised modeling using Random Forest

The repository is organized into **5 notebooks**, which implement a complete pipeline from raw transaction data to the final anomaly output file.

---

## 1. Dataset Overview

The original dataset is located at:

```text
data/bank_transactions_data.csv
````

It contains **2,512 records** with core transaction-related fields, including transaction dates, transaction amounts, merchants, devices, login attempts, account balance, and other banking activity features.

During the pipeline, enriched versions of the dataset are created with additional engineered features and anomaly flags.

---

## 2. Workflow

### `notebooks/01_data_understanding_cleaning.ipynb`

Main steps:

1. Load the original dataset.
2. Perform data quality checks:

   * `shape`
   * `describe`
   * `isnull`
   * `nunique`
3. Remove duplicate records.
4. Convert date columns:

   * `TransactionDate`
   * `PreviousTransactionDate`
5. Apply label encoding to categorical variables.
6. Apply MinMax scaling to numerical features.
7. Create additional features, such as:

   * `time_diff`
   * `location_change`
8. Export the cleaned dataset to:

```text
data/clean_transactions_data.csv
```

---

### `notebooks/02_exploratory_data_analysis.ipynb`

Main steps:

1. Perform EDA on the cleaned dataset.
2. Analyze distributions and outliers.
3. Create visualizations:

   * scatterplots
   * boxplots
   * hourly activity plots
   * correlation heatmaps
4. Create rule-based suspicious behavior flags, such as:

   * `high_amount_flag`
   * `many_login_attempts_flag`
   * `suspicious_merchant_flag`
   * `reactivation_suspect_flag`
   * `amount_exceeds_balance`
5. Export the dataset with EDA-based flags to:

```text
data/clean_transactions_with_flags.csv
```

---

### `notebooks/03_statistical_methods.ipynb`

Main steps:

1. Calculate z-scores for selected numerical features.
2. Create binary statistical flags using the rule:

```text
|z| > 3
```

3. Merge the new z-score flags with the existing dataset.
4. Export the enriched dataset to:

```text
data/transactions_with_zscores.csv
```

---

### `notebooks/04_isolation_forest_model.ipynb`

Main steps:

1. Train an Isolation Forest model for unsupervised anomaly detection.
2. Create the field:

```text
isolation_outlier_label
```

3. Combine multiple anomaly indicators into a final anomaly flag:

```text
final_anomaly_flag
```

4. Export the final dataset with all combined flags to:

```text
data/transactions_with_all_flags_final.csv.csv
```

---

### `notebooks/05_supervised_anomaly_model.ipynb`

Main steps:

1. Perform a train/test split using stratification.
2. Train a `RandomForestClassifier`.
3. Evaluate the model using:

   * classification report
   * confusion matrix
4. Analyze feature importance.
5. Create the final dataset containing only anomalous transactions.
6. Export the final anomalies-only file to:

```text
transactions_anomalies_only.csv
```

---

## 3. Data Artifacts

The pipeline generates the following files:

| File                                             | Description                                           | Shape                  |
| ------------------------------------------------ | ----------------------------------------------------- | ---------------------- |
| `data/bank_transactions_data.csv`                | Original dataset                                      | 2,512 rows, 16 columns |
| `data/clean_transactions_data.csv`               | Cleaned and feature-engineered dataset                | 2,512 rows, 18 columns |
| `data/clean_transactions_with_flags.csv`         | Dataset with EDA-based flags                          | 2,512 rows, 31 columns |
| `data/transactions_with_zscores.csv`             | Dataset with statistical z-score flags                | 2,512 rows, 47 columns |
| `data/transactions_with_all_flags_final.csv.csv` | Final dataset with all combined anomaly flags         | 2,512 rows, 50 columns |
| `transactions_anomalies_only.csv`                | Final extract containing only suspicious transactions | 947 rows, 28 columns   |

Final label distribution in the complete dataset using `final_anomaly_flag`:

| Label | Meaning                   | Count |
| ----- | ------------------------- | ----: |
| `0`   | Non-anomalous transaction | 1,565 |
| `1`   | Anomalous transaction     |   947 |

---

## 4. Recommended Environment

Minimum required Python libraries:

* `pandas`
* `numpy`
* `matplotlib`
* `seaborn`
* `scipy`
* `scikit-learn`
* `jupyter`

Example installation command:

```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn jupyter
```

Optional `requirements.txt`:

```text
pandas
numpy
matplotlib
seaborn
scipy
scikit-learn
jupyter
```

---

## 5. How to Run the Pipeline

Run the notebooks in the following order:

1. `notebooks/01_data_understanding_cleaning.ipynb`
2. `notebooks/02_exploratory_data_analysis.ipynb`
3. `notebooks/03_statistical_methods.ipynb`
4. `notebooks/04_isolation_forest_model.ipynb`
5. `notebooks/05_supervised_anomaly_model.ipynb`

Before running the pipeline, make sure that the relative paths remain unchanged, for example:

```text
../data/...
```

Each notebook should be completed successfully before moving to the next one.

---

## 6. Notes and Observations

* The file `transactions_with_all_flags_final.csv.csv` has a duplicated `.csv.csv` extension. It works normally, but it can optionally be renamed for clarity.
* This project can serve as a strong foundation for:

  * threshold tuning
  * cost-sensitive learning
  * explainability on flagged transactions
  * comparison with more specialized anomaly detection models

---

## 7. Short Summary

This repository implements an end-to-end fraud detection workflow for bank transactions.

The pipeline starts with data cleaning and feature engineering, continues with exploratory data analysis and rule-based suspicious behavior flags, then applies statistical methods, unsupervised anomaly detection, and supervised classification.

The final output is a dataset containing transactions identified as potentially suspicious.

```
```
