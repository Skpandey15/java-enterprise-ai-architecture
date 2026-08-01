# 16. NumPy, pandas and scikit-learn Hands-On

This chapter builds the classical Python data workflow required before deep learning or GenAI orchestration.

## Tool map

| Tool | Purpose |
|---|---|
| Jupyter | Reproducible exploration |
| NumPy | Vectorised numerical arrays |
| pandas | Tabular cleaning, joins and aggregation |
| Matplotlib/Seaborn | Distribution and error visualisation |
| scikit-learn | Preprocessing, models, pipelines and metrics |

## Core workflow

1. Inspect schema, units, nulls, duplicates and target distribution.
2. Correct types explicitly and validate joins to prevent row multiplication.
3. Split before learning preprocessing parameters.
4. Put imputation, encoding and scaling inside a scikit-learn `Pipeline`.
5. Compare with a naive baseline and cross-validate appropriately.
6. Save the code, data version, seed, metrics and environment.

```python
from sklearn.compose import ColumnTransformer
from sklearn.impute import SimpleImputer
from sklearn.linear_model import LogisticRegression
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import OneHotEncoder, StandardScaler

numeric = Pipeline([
    ("impute", SimpleImputer(strategy="median")),
    ("scale", StandardScaler()),
])
categorical = Pipeline([
    ("impute", SimpleImputer(strategy="most_frequent")),
    ("encode", OneHotEncoder(handle_unknown="ignore")),
])
features = ColumnTransformer([
    ("num", numeric, ["experience", "score"]),
    ("cat", categorical, ["role", "location"]),
])
model = Pipeline([
    ("features", features),
    ("classifier", LogisticRegression(class_weight="balanced", max_iter=1000)),
])
```

The pipeline learns preprocessing only from training folds and applies identical logic in production.

## Explainability

Use coefficients, permutation importance or SHAP carefully. Explanations describe model behaviour, not causality. Validate stability and avoid exposing sensitive features.

## Portfolio lab

Build an interview-outcome model:

- Create a data-quality report and leakage-safe pipeline.
- Handle imbalance and select a threshold from business costs.
- Test serialisation and inference schema.
- Expose `/predict` and `/health` with FastAPI.
- Log model version and latency without logging PII.
- Publish intended and prohibited uses in a model card.

## Common failures

- Cleaning manually in a notebook but not in production
- Encoding before the split
- Randomly splitting time-series data
- Treating missing values as zero without domain meaning
- Ignoring calibration, latency and segment-level errors
- Loading untrusted pickle/joblib files, which may execute code

You are ready when another developer can reproduce the metrics, run tests, start the API and understand where the model must not be used.
