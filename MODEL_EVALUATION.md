# Model selection and held-out evaluation

Four Hydra configurations compared max_depth values 10 and 50 with n_estimators values 100 and 200. Lowest validation MAE selected random_forest_export:v3: 200 trees, maximum depth 50, random seed 42. This model has the prod alias. Selection preceded held-out evaluation.

| Dataset | Rows | MAE (dollars per night) | R2 |
| --- | ---: | ---: | ---: |
| Validation | 3040 | 34.184256 | 0.550616 |
| Held-out test | 3801 | 33.846924 | 0.561929 |

Test MAE changed by -0.987% relative to validation. It is slightly lower, with similar R2, indicating comparable performance on this held-out split. Held-out scores were not used to retune the model.

The split contains 15200 train/validation listings and 3801 test listings with no overlapping IDs. The former splits into 12160 training and 3040 validation rows.

The provided test_regression_model component consumed random_forest_export:prod and test_data.csv:latest and finished successfully. Its actual MLproject data parameter is test_dataset. Held-out evaluation remains excluded from the default pipeline and runs explicitly.

Evaluation run: https://forge.coreweave.com/wandb/justinnull-western-governors-university/nyc_airbnb/runs/sfy1hyph

Selected model: https://forge.coreweave.com/wandb/justinnull-western-governors-university/nyc_airbnb/artifacts/model_export/random_forest_export/v3
