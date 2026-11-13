# Reviewer corrections for resubmission

All four requested corrections from the review of the November 12 submission have been implemented and checked against new W&B runs. The earlier rubric review's interpretation of artifact naming and its screenshot evidence were insufficient; this document supersedes that interpretation.

| Reviewer item | Correction | Verified evidence |
| --- | --- | --- |
| Q5: cleaned artifact name | Cleaner saves and uploads `clean_data.csv`. Main uses that name for output, validation input/reference, and splitting. | New `clean_data.csv:v0` contains `clean_data.csv`, 19,001 rows. Six data checks passed. |
| Q6: own reference screenshot | Applied `reference` manually in the user's W&B UI to the original renamed baseline. | Replaced `images/wandb-tag-data-test.png` with a new screenshot showing `justinnull-western-governors-university`, `clean_data.csv:v0`, and `reference`. |
| Q13: model collection name | Main outputs `model_export` of type `model_export`; explicit testing uses `model_export:prod`. Reran all four Hydra comparisons. | New `model_export:v0` through `v3`; minimum validation MAE selected `v3`, carrying `prod`. Held-out run `p7llvwgm` finished with MAE $33.846924 and R2 0.561929. |
| Q17: own pipeline screenshot | Opened the renamed clean dataset's Lineage view in the submitted W&B account. | Replaced `images/wandb-pipeline-graph.png` with the account breadcrumb and actual raw-data, cleaning, validation, split, four training/export, and held-out branches. |

## Reproducible verification

- All four new comparison runs finished, using the same `trainval_data.csv:v4`. Depths are 10/50; tree counts are 100/200. The lowest validation MAE is $34.184256 from `model_export:v3`, run `i7ayn7af`.
- Original-sample row counts: 19,001 clean; 15,200 train/validation; 3,801 held-out. The train/validation and held-out ID sets have zero overlap.
- The held-out audit checked actual model and dataset artifact IDs, not just names or latest aliases. Selection preceded held-out testing.
- The fixed reference is `clean_data.csv:v0`. It is not moved when new samples are processed.
- Both collections use reviewer-required names. The former `clean_sample.csv` and `random_forest_export` collections remain historical records.
- The main VS Code workspace crashed during restoration. The reruns use the course's working notebook workspace with the same pinned component environments and source, rather than claiming completion without execution. `environment.yml` now includes Git, required for remote MLflow release execution.

[Machine-readable run, artifact, and metric evidence](RESUBMISSION_RESULTS.json) and [the held-out comparison](MODEL_EVALUATION.md) accompany the screenshots.

The corrected release is 1.0.4. Its exact remote execution on `sample2.csv` is checked separately after publication. The previously accepted baseline-failure exercise remains in immutable releases 1.0.2 and 1.0.3.

The project has not been resubmitted. Grading remains the reviewer's decision.
