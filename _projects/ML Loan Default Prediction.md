---
title: "ML: Loan Default Prediction"
date: 2022-05-27
status: "Completed"
field: "ML"
tools:
  - Python
  - Pandas
  - Scikit-learn
github:
demo:
kaggle: https://www.kaggle.com/code/ma12492002/ml-loan-default-prediction/notebook
excerpt: "This is a Kaggle competition notebook that builds a machine learning classifier to predict loan default outcomes (i.e., whether a loan will result in a loss, and how much). It was submitted to the Loan Default Prediction competition on Kaggle."
---

Here's a comprehensive description of the project:


---

### Dataset
- **Train set:** `train_v2.csv.zip` — indexed by `id`, contains loan features and a `loss` target column.
- **Test set:** `test_v2.csv.zip` — same features, no target; used for final predictions.
- **Sample submission:** A template CSV that defines the required output format.

The data is high-dimensional with many columns, and both train and test sets contain **missing values** across multiple features.

---

### Pipeline & Methodology

**1. Data Loading & Exploration**
- Loads both train and test datasets with pandas.
- Inspects shape, dtypes, and missing value counts using a custom `df_nulls()` function that reports both the count and percentage of nulls per column.

**2. Feature Selection — Numeric Only**
- Filters down to **only numeric columns**, dropping all categorical/object-type features entirely. This is a simplifying design choice.
- Drops rows where the `loss` target is null from the training set.

**3. Train/Validation Split**
- An 80/20 split using `train_test_split` (with `random_state=42` for reproducibility).

**4. Preprocessing Pipeline**
- Uses a **scikit-learn `Pipeline`** with two steps:
  - `SimpleImputer` (mean strategy) — fills missing values.
  - `StandardScaler` — normalizes numeric features.
- Wrapped in a `ColumnTransformer` for clean, leak-free application.

**5. Feature Importance via Permutation Importance**
- Trains a **Logistic Regression** model (used as a helper model, not the final one).
- Uses `eli5`'s `PermutationImportance` on a 1,000-sample validation subset to rank features by their actual impact on predictions.
- Applies `SelectFromModel` with a threshold of `0.001` to filter down to only the most predictive features.

**6. Model Training — Random Forest Classifier**
- The final model is a `RandomForestClassifier` with:
  - `n_estimators=10` (10 trees — relatively small)
  - `criterion='entropy'`
- Evaluated using **5-fold cross-validation**, with mean accuracy reported.
- After validation, the model is **retrained on the full training set** for improved generalization before generating test predictions.

**7. Submission**
- Predictions are written into the `submission.csv` file in the format required by the Kaggle competition.

---

### Key Design Choices & Observations

| Aspect | Detail |
|---|---|
| Target variable | `loss` — a binary/multiclass classification target |
| Feature engineering | None; raw numeric features only |
| Imputation | Mean imputation (could miss non-MAR patterns) |
| Feature selection | Permutation importance via a proxy Logistic Regression |
| Final model | Random Forest (small forest with 10 estimators) |
| Evaluation metric | Cross-validation accuracy |
| Categorical features | Dropped entirely |

---


## Links

[Kaggle](https://www.kaggle.com/code/ma12492002/ml-loan-default-prediction/notebook)