# RGB training modules

| Module | Responsibility |
|---|---|
| `dataset.py` | Parses metadata splits and loads sampled RGB frame folders |
| `model.py` | Builds torchvision Swin3D variants and replaces their heads |
| `trainer.py` | Differential learning rates or progressive unfreezing, validation, and weight selection |
| `evaluator.py` | Top-k test evaluation, confusion matrix, and text report |
| `utils.py` | Configuration loading, seeds, console logging, experiment directories, and top-k calculation |

The repository-root `train.py` orchestrates these modules. This folder does not train the landmark ConvBiLSTM. That implementation remains in the main notebook; the app's inference implementation is in `demo/vision_model.py`.
