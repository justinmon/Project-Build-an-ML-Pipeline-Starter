# Model selection and held-out evaluation - resubmission

Four new Hydra jobs use collection `model_export` and artifact type `model_export`. All use `trainval_data.csv:v4`, seed 42, the same borough-stratified split, and identical remaining configuration.

| Depth | Trees | Artifact | Validation MAE | Validation R2 | Run |
| ---: | ---: | --- | ---: | ---: | --- |
| 10 | 100 | model_export:v0 | 34.431949 | 0.545613 | sifxo00k |
| 10 | 200 | model_export:v1 | 34.404576 | 0.546276 | o8ip6sry |
| 50 | 100 | model_export:v2 | 34.196322 | 0.550076 | qcqtfg92 |
| 50 | 200 | **model_export:v3** | **34.184256** | **0.550616** | i7ayn7af |

Lowest validation MAE selected `model_export:v3`: 200 trees, maximum depth 50. Its `prod` alias was applied before held-out evaluation. The export contains `MLmodel`, `model.pkl`, environment files, and input examples.

| Dataset | Rows | MAE ($/night) | R2 |
| --- | ---: | ---: | ---: |
| Validation | 3,040 | 34.184256 | 0.550616 |
| Held-out test | 3,801 | 33.846924 | 0.561929 |

Held-out MAE is 0.987% lower than validation, with comparable R2. This supports comparable performance on this split. Test results were not used for selection or further tuning.

The 19,001-row baseline is `clean_data.csv:v0`, carrying `reference`. The first split has 15,200 train/validation rows and 3,801 test rows, with zero overlapping IDs. The inner split has 12,160 training and 3,040 validation rows.

The provided test component consumed selected `model_export:prod` and `test_data.csv:v4` and finished successfully. An audit checked the actual artifact IDs against selection and splitting. Held-out evaluation remains excluded from default training.

- [Selection run](https://forge.coreweave.com/wandb/justinnull-western-governors-university/nyc_airbnb/runs/i7ayn7af)
- [Evaluation run](https://forge.coreweave.com/wandb/justinnull-western-governors-university/nyc_airbnb/runs/p7llvwgm)
- [Selected model](https://forge.coreweave.com/wandb/justinnull-western-governors-university/nyc_airbnb/artifacts/model_export/model_export/v3)
- [Recorded results and artifact IDs](RESUBMISSION_RESULTS.json)

Historical `random_forest_export` results remain in earlier GitHub releases and W&B. Results above come from new training runs under `model_export`.
