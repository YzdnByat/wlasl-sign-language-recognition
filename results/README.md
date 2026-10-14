# Recorded validation metrics

`robust-convbilstm-validation.csv` was parsed from the saved output in code cell 18 of `notebooks/landmark_pipeline.ipynb`. It has one row per logged epoch:

- `epoch`
- `train_accuracy_percent`
- `validation_top1_percent`
- `validation_top5_percent`

These are historical printed values rounded to two decimal places. They are validation results, not a new test evaluation or rerun. See [results provenance](../docs/results.md) for the first best-Top-1 epoch and the independent Top-5 peak.
