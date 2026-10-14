# Reproduction guide

## Environment

The original dependency file requests PyTorch 2.7+ and torchvision 0.22+, but does not record a complete training-environment lockfile. Use a separate environment and an appropriate PyTorch build from the [official selector](https://pytorch.org/get-started/locally/). The original RGB training code uses CUDA-specific AMP; its CPU fallback is not validated here.

```bash
python -m venv .venv
# Activate the environment with the command for your shell.
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

`requirements.txt` covers the RGB experiment. Use `requirements-demo.txt` for the Gradio demo. Colab notebooks include their own package-installation cells and expect Google Drive.

## WLASL inputs

Follow the [original dataset preparation instructions](https://github.com/dxli94/WLASL). Obtain processed videos and `WLASL_v0.3.json` separately. Use filenames matching metadata video IDs. Record the metadata version and IDs actually available locally.

For the default RGB paths, place videos directly in `videos/` and the metadata at the repository root as `WLASL_v0.3.json`. The extraction script scans direct `.mp4` and `.avi` children, not a recursive directory tree.

## RGB frame extraction

With default paths:

```bash
python scripts/extract_yolo.py
```

This extracts up to the requested sampled positions, crops a detected person where available, and writes `frame_00.jpg` onward under each video folder in `extracted_frames_yolo/`. Short or unreadable videos can produce fewer than 32 frames. The current loader substitutes zero tensors for absent frame files, so inspect extraction completeness before interpreting results.

For custom paths, from the repository root:

```bash
python -c 'from scripts.extract_yolo import extract_frames_yolo; extract_frames_yolo(video_dir="/absolute/path/to/videos", output_dir="/absolute/path/to/frames", num_frames=32)'
```

That example uses a POSIX shell. On Windows, edit the function's call at the bottom of the script or invoke it in a Python session. The extraction script does not read the experiment YAML.

## RGB training and outputs

Edit `extracted_frames_dir`, `wlasl_json_path`, `output_dir`, and `experiment_name` in a YAML. Use a new experiment name to avoid overwriting a previous run.

```bash
python train.py --config configs/base.yaml
```

Supported strategies are `paper_end2end` and `progressive`. The commented `head_only` option in the original configuration is not a supported dispatch option in `train.py`; the progressive strategy includes a head-only phase.

The code writes to `output_dir/experiment_name/`:

| File | Purpose |
|---|---|
| `config_used.yaml` | Configuration copy saved at startup, before the dataset loader adjusts the class count |
| `training.log` | Printed run output |
| `best_paper_model.pth` or `best_progressive_model.pth` | Model weights selected by validation Top-1 |
| `confusion_matrix.png` | Test confusion-matrix figure when predictions are available |
| `test_classification_report.txt` | Test Top-1/Top-5 and classification report |

The plotting helper for loss/accuracy curves exists in `src/evaluator.py`, but `train.py` does not call it. This interface does not provide resumable optimizer state or a saved predictions CSV. A class-mapping export is not currently part of the training entry point.

The checkpoint-selection code starts with a best score of zero. If no epoch improves beyond zero, no checkpoint is written and the final load will fail. This is existing training behavior, not changed by the documentation cleanup.

## Landmark notebook

Open `notebooks/landmark_pipeline.ipynb` in Colab. Its original path variables assume:

- `PROJECT_DIR`: `/content/drive/MyDrive/SignLanguageProject`
- Dataset archive: `wlasl-processed.zip` within that directory
- Temporary extraction: `/content/wlasl_raw`

The archive must contain the expected WLASL metadata and processed video files. Run the setup, metadata parsing, landmark extraction, and preprocessing cells before model experiments. The base ConvBiLSTM training runs inside code cell 7, so running that cell also performs its training loop. The later RobustConvBiLSTM experiment reuses the file lists and labels defined there.

The processing cell interpolates internal missing-hand runs, normalizes coordinates relative to the nose and shoulder distance, and adds temporal differences. The final dataset pads or center-crops to 40 frames. Inspect dataset availability and all-class coverage before starting a new run.

The archival notebooks are earlier experimental routes with their own Drive paths and assumptions. They are preserved for provenance, not required dependencies of the main notebook.

## Validation scope

No GPU training or full application launch was performed during the repository cleanup. The static checks validate syntax, local references, and artifact preservation. They do not validate the dataset, recognize signs, or reproduce the recorded accuracy.
