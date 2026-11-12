# Reproducible NYC Short-Term Rental Price Pipeline

This project builds a repeatable pipeline for estimating nightly rental prices in New York City. MLflow runs isolated components, Hydra manages configuration and parameter comparisons, and Weights & Biases versions datasets, models, metrics, and lineage.

- [GitHub repository](https://github.com/justinmon/Project-Build-an-ML-Pipeline-Starter)
- [W&B project](https://forge.coreweave.com/wandb/justinnull-western-governors-university/nyc_airbnb)
- [Selected model](https://forge.coreweave.com/wandb/justinnull-western-governors-university/nyc_airbnb/artifacts/model_export/random_forest_export/v3)

## Pipeline

1. `download`: version the incoming sample as `sample.csv`.
2. `basic_cleaning`: retain prices from $10 through $350, convert review dates, and retain longitude -74.25 through -73.50 and latitude 40.5 through 41.2; upload `clean_sample.csv`.
3. `data_check`: check columns, borough names, geographic bounds, distribution drift, row count, and price range. The `reference` alias stays pinned to the original clean dataset.
4. `data_split`: reserve 20% for held-out testing; upload `trainval_data.csv` and `test_data.csv`.
5. `train_random_forest`: split the remainder into training and validation data, fit preprocessing and regression together, record MAE/R2, and export an MLflow model to W&B.
6. `test_regression_model`: explicitly evaluate a selected model carrying the `prod` alias. This step is excluded from default training.

The EDA notebook is executed and saved at `src/eda/eda.ipynb`. Missing values are handled inside the inference pipeline. Categorical values use imputation and encoding, numeric inputs use constant imputation, review dates become elapsed-day features, and listing names use TF-IDF.

## Setup

Use Linux or macOS with Conda/Miniforge. The project targets Python 3.13. Components pin their dependencies separately.

```bash
git clone https://github.com/justinmon/Project-Build-an-ML-Pipeline-Starter.git
cd Project-Build-an-ML-Pipeline-Starter
conda env create -f environment.yml
conda activate nyc_airbnb_dev
export WANDB_BASE_URL=https://api.forge.coreweave.com
wandb login --verify
```

The development environment uses W&B 0.31.0 for the current Forge login flow. Validated component environments retain W&B 0.24.0. `WANDB_BASE_URL` makes both use the same endpoint. For a nonstandard Conda installation, set `MLFLOW_CONDA_HOME` to its installation directory. Keep API keys outside the repository. To log to another W&B team, set `WANDB_ENTITY` to its slug.

## Run and compare models

```bash
# Download, clean, check, split, and train using config.yaml.
mlflow run .

# Run one component through the pipeline.
mlflow run . -P steps=data_check

# Four configurations used for model selection.
mlflow run . -P steps=train_random_forest \
  -P hydra_options="modeling.random_forest.max_depth=10,50 modeling.random_forest.n_estimators=100,200 -m"

# After promoting a selected model to prod in W&B.
mlflow run . -P steps=test_regression_model
```

Split sizes, random seed, price bounds, drift threshold, and forest parameters are in `config.yaml`. Defaults use the selected 200-tree forest with maximum depth 50. Splitting and training use seed 42 and borough stratification.

## Model selection and held-out results

The original sample has 20,000 listings; price cleaning retained 19,001. The split contains 12,160 training rows, 3,040 validation rows, and 3,801 held-out test rows, with no training/test ID overlap.

| Maximum depth | Trees | Validation MAE ($/night) | Validation R2 |
| ---: | ---: | ---: | ---: |
| 10 | 100 | 34.431949 | 0.545613 |
| 10 | 200 | 34.404576 | 0.546276 |
| 50 | 100 | 34.196322 | 0.550076 |
| 50 | 200 | **34.184256** | **0.550616** |

Selection used validation MAE. Model `random_forest_export:v3` received `prod` before held-out evaluation. Test MAE was **$33.846924/night**, with **R2 0.561929**: about 0.99% lower MAE than validation, indicating comparable performance on this split. Held-out results were not used to retune the model.

The export reloaded through both MLflow sklearn and pyfunc interfaces and reproduced validation metrics. See [the evaluation report](MODEL_EVALUATION.md) and [recorded test results](EVALUATION_RESULTS.json). The model artifact's **Lineage** view in W&B shows data-to-model-to-test connections.

## Release validation

The initial `1.0.0` release was found to contain an incomplete orchestration file: cleaning and data-check branches were placeholders. Although its execution completed, it reused the previous clean dataset, so that run is **not valid evidence** of new-sample validation. The tag is retained to preserve an honest release history.

The cleaning arguments, both missing data tests, and orchestration were restored in commit `9a6dfb2ec358ab16230905901363dbe2855721db`. The baseline geographic-check exercise uses this immutable revision:

```bash
mlflow run https://github.com/justinmon/Project-Build-an-ML-Pipeline-Starter.git \
  -v 9a6dfb2ec358ab16230905901363dbe2855721db -P hydra_options="etl.sample='sample2.csv'"
```

The restored baseline produced **five passing tests and one failure**, `test_proper_boundaries`: exactly one listing was outside the accepted region. The pipeline stopped before splitting or training, as required. MLflow parent run: `1c82362fe4f9460195083abe324d2383`.

Release `1.0.1` adds the matching inclusive longitude/latitude filter before saving the cleaned data. Run the complete updated release with:

```bash
mlflow run https://github.com/justinmon/Project-Build-an-ML-Pipeline-Starter.git \
  -v 1.0.1 -P hydra_options="etl.sample='sample2.csv'"
```

The exact command completed successfully (exit status 0), with **all six data checks passing**. The new sample contained 48,895 rows; price and geographic cleaning retained 46,427. The new model is `random_forest_export:v5`, produced by [run ff4s2rc1](https://forge.coreweave.com/wandb/justinnull-western-governors-university/nyc_airbnb/runs/ff4s2rc1), with validation MAE **$32.427103/night** and R2 **0.578117**. These are validation results for the updated sample, not a repeat of the earlier held-out evaluation.

The artifact chain was verified through W&B: `sample.csv:v1` → `clean_sample.csv:v2` → `trainval_data.csv:v2` → `random_forest_export:v5`. The original `reference` alias remains on clean dataset `v0`, and `prod` remains on selected model `v3`. The new export contains the model, MLflow metadata, environment files, and input examples. See [release validation results](RELEASE_VALIDATION.json).

## Possible improvements

Future work could compare other regressors, use richer listing-text features, investigate neighborhood-level errors, and monitor drift and performance over time.

Based on the [Udacity WGU starter project](https://github.com/udacity/Project-Build-an-ML-Pipeline-Starter). The original license is retained.
