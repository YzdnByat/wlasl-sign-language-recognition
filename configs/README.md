# RGB experiment configurations

| YAML | Intended use |
|---|---|
| `base.yaml` | Default RGB/Swin experiment with differential learning rates |
| `laptop_4050.yaml` | Original smaller-batch laptop settings |
| `kaggle.yaml` | Original Kaggle paths and settings |
| `test_dummy.yaml` | Two-class synthetic-data plumbing configuration |

Run from the repository root with `python train.py --config configs/base.yaml`. Update data paths and use a distinct `experiment_name` for each run. Configuration hardware comments describe the intended original setup; they are not a verified memory or performance guarantee.

`train.py` accepts `paper_end2end` and `progressive`. The original YAML comment also lists `head_only`, but that value is not implemented in the dispatcher. The synthetic generator does not supply each class in every split, so its output is not an accuracy benchmark or a guaranteed end-to-end smoke test.
