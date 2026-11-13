# Rubric review after reviewer corrections

This review supersedes the first submission's naming and screenshot interpretation. See [RESUBMISSION_CORRECTIONS.md](RESUBMISSION_CORRECTIONS.md) for Q5, Q6, Q13, and Q17, which were the four items returned for changes.

| Area | Evidence |
| --- | --- |
| Public W&B and GitHub links | Both are in README and were accepted by the reviewer. W&B project `nyc_airbnb` is public. Include both links again in Submission Details. |
| Sample and EDA | `sample.csv` artifact exists. Executed notebook has seven executed code cells and zero errors. This area was accepted. |
| Cleaning parameters | Four string arguments and two floating-point price bounds have descriptions; numeric limits come from configuration. |
| Cleaning and exact artifact name (Q5) | `main.py` and `src/basic_cleaning/run.py` use `clean_data.csv`, including its saved/uploaded filename and downstream references. New original-sample artifact `clean_data.csv:v0` is present. |
| Fixed reference and own screenshot (Q6) | Original `clean_data.csv:v0` has `reference`, applied in W&B UI. `images/wandb-tag-data-test.png` shows that alias and the submitted account breadcrumb. |
| Row-count and price tests | Both added tests are implemented. All six tests passed on the renamed baseline. |
| Splitting | Supplied component uses configured fractions, seed 42, and borough stratification. Train/validation and test artifacts both exist; 15,200 and 3,801 rows, zero overlapping IDs. |
| Inference pipeline | Categorical imputation/one-hot encoding, combined preprocessing/random forest, fit, MLflow sklearn export, and MAE logging are implemented. This area was accepted. |
| Exact model collection name (Q13) | Four new artifacts `model_export:v0` through `v3`, each of type `model_export`, contain MLflow model packages. Testing references `model_export:prod`. |
| Hydra comparison | Four finished jobs compare depth 10/50 and trees 100/200 on identical `trainval_data.csv:v4`. Run IDs and actual metrics are recorded in RESUBMISSION_RESULTS.json. |
| Model selection | Lowest validation MAE $34.184256 selected `model_export:v3`; `prod` applied before held-out evaluation. |
| Held-out testing | New finished run `p7llvwgm` consumes exactly selected model `v3` and `test_data.csv:v4`. MAE $33.846924 and R2 0.561929 are comparable to validation. Test results did not select or retune the model. |
| Own lineage screenshot (Q17) | `images/wandb-pipeline-graph.png` shows the user's account and actual cleaned-data lineage, including data checks, split, four new training/export paths, and held-out evaluation. |
| Release/update exercise | Previously accepted baseline release 1.0.2 fails the geographic check on one new-sample listing; corrected 1.0.3 succeeds. New corrected-name release 1.0.4 is also remotely verified after publication. |
| Geographic filtering | Inclusive longitude [-74.25,-73.50] and latitude [40.5,41.2] filters remain implemented. |
| Future improvements | README discusses alternative models, text features, neighborhood errors, and monitoring. |

The two instructor screenshots have been replaced at the exact filenames cited by the reviewer. The remaining starter GIF is explicitly labeled in README as a course example, not personal execution evidence. The new screenshots and manifests should be reviewed alongside the actual public W&B project.

This is evidence of the implementation and completed checks, not a guarantee of grading. The project is not resubmitted automatically.
