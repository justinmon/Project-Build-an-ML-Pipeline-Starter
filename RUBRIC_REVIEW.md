# Final rubric review

Reviewed against the Udacity project rubric and submission instructions on November 12, 2026. The required implementation and execution evidence are present. Grading remains the reviewer's decision. The final release is **1.0.3**; release **1.0.2** is the deliberately failing baseline used for the release-update exercise.

| Requirement | Evidence and result |
| --- | --- |
| Public W&B project and both links | `nyc_airbnb` visibility is Public. README includes both GitHub and W&B project links. Both links must also be pasted into Submission Details by the submitter. |
| Obtain and upload sample data | W&B `sample.csv` raw-data artifact exists; original sample and new `sample2.csv` were fetched by the download component. |
| EDA | `src/eda/eda.ipynb` preserves seven executed code cells with zero errors, descriptive statistics, dataset inspection, and price/date cleaning. |
| Typed cleaning parameters with documentation | All six argparse arguments have descriptions; four use `str`, and price limits use `float`. |
| Cleaning called by main | `main.py` calls the cleaning component; min/max prices come from `config.yaml`. Final release executes it without errors. |
| Cleaned artifact | `clean_sample.csv` is uploaded with type `clean_sample`. The new sample retains 46,427 of 48,895 rows after price and location filtering. |
| Reference alias | `clean_sample.csv:reference` remains pinned to original `v0`, preserving the original drift reference. |
| Two added tests | `test_row_count` requires 15,000 < rows < 1,000,000. `test_price_range` checks every price against inclusive configured bounds. |
| All data tests pass | Final release 1.0.3: six passed, zero failed; nine dependency deprecation warnings do not affect the result. |
| Split component | The supplied split component is called with configured test fraction, seed, and stratification. Both `trainval_data.csv` and `test_data.csv` artifacts exist. |
| Random forest preprocessing | Most-frequent categorical imputation plus one-hot encoding; the final inference pipeline contains preprocessing and the random forest. |
| Fit, MLflow export, MAE logging | The pipeline is fitted, saved with `mlflow.sklearn.save_model`, and MAE/R2 are recorded. Comments identify each required implementation step. |
| Training called by main | Training is called with the serialized forest configuration and configured split/features parameters; the full release succeeds. |
| Model export artifact | Artifact type `model_export`, collection `random_forest_export`. Final model `v6` contains MLmodel, model.pkl, conda.yaml, requirements, Python environment, and input examples. |
| More than two parameter comparisons | Four finished Hydra training jobs compare depths 10/50 and tree counts 100/200, all using original `trainval_data.csv:v0`. Run states, metrics, and artifacts were rechecked through W&B. |
| Best model promoted | Within the original tuning cohort, `random_forest_export:v3` has the lowest validation MAE ($34.184256) and carries `prod`. Later updated-sample metrics are a different data comparison and were not used to retune against held-out data. |
| Held-out evaluation | Finished run `sfy1hyph` uses model `v3` and `test_data.csv:v0`. Test MAE $33.846924 and R2 0.561929 are comparable to validation; test MAE is about 0.99% lower. The explicit test component is implemented and excluded from default training. |
| Pipeline visualization | W&B artifact Lineage was inspected for the selected model, showing raw data, cleaning, splitting, training, model export, and held-out testing. |
| GitHub release | Published releases include final 1.0.3 at commit `3e42b745a681ea8fb148965c9ac9d17cdec25e86`. The rubric permits additional trial version numbers. |
| Released baseline fails on new sample | Published 1.0.2 targets restored pre-filter commit `9a6dfb2ec358ab16230905901363dbe2855721db`. Remote execution on `sample2.csv` exits 1: five checks pass and exactly one point fails `test_proper_boundaries`. Training is blocked. |
| Geographic cleaning added | Cleaning retains longitude [-74.25, -73.50] and latitude [40.5, 41.2] before saving. |
| Updated release and successful new model | Remote execution of subsequent release 1.0.3 on `sample2.csv` exits 0 and produces new model `random_forest_export:v6`, W&B run `h9rhljch`. All six checks pass. |
| Optional improvements discussion | README describes future model comparisons, richer text features, neighborhood errors, and monitoring. |

## Naming and release-history notes

Step 2 explicitly requires `clean_sample.csv`, as do subsequent rubric sections and the supplied components. One cleaning rubric row says `clean_data.csv`; the implementation follows the consistent project instructions. Similarly, `model_export` is the required artifact type, while the configured collection is `random_forest_export`.

Initial release 1.0.0 was incomplete and is documented accordingly; it is retained unchanged. Release 1.0.1 successfully ran the corrected pipeline. The final review added a published pre-filter baseline 1.0.2 and a subsequent corrected release 1.0.3 so both sides of the release exercise use release tags. The executed EDA notebook was restored before final release 1.0.3. No release history was rewritten.

## Submission actions remaining

The project has **not** been submitted. Udacity currently shows **Connect With GitHub**. The user must connect their account, select the public repository, add both links below to Submission Details, and submit when ready:

- GitHub: https://github.com/justinmon/Project-Build-an-ML-Pipeline-Starter
- Public W&B project: https://forge.coreweave.com/wandb/justinnull-western-governors-university/nyc_airbnb
- Final release: https://github.com/justinmon/Project-Build-an-ML-Pipeline-Starter/releases/tag/1.0.3
