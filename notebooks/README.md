# Landmark experiment notebooks

| Notebook | Role |
|---|---|
| [`landmark_pipeline.ipynb`](landmark_pipeline.ipynb) | Main Colab record: dataset setup, top-100 selection, MediaPipe extraction, feature processing, base ConvBiLSTM, pose-graph baseline, and RobustConvBiLSTM |
| [`archive/data_preparation.ipynb`](archive/data_preparation.ipynb) | Earlier data audit, interpolation, normalization, and mirroring experiments |
| [`archive/sequence_experiments.ipynb`](archive/sequence_experiments.ipynb) | Earlier sequence-model and split experiments |

These are the original notebooks, moved without editing cells, stored outputs, or metadata. Their historical Drive paths must be updated for another account. The main notebook is an experiment record with training embedded in setup/model cells; it is not a command-line script.

Read [reproduction instructions](../docs/reproduction.md) before execution and [results provenance](../docs/results.md) before quoting metrics. Empty code cells and earlier blocks are retained to preserve the original experiment record.
