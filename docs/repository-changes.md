# Repository organization changes

Source repository: `YzdnByat/Benchmark-Skeleton-Sequence-Modeling-for-Isolated-Sign-Language-Recognition-on-WLASL`  
Inspected source commit: `b2521db07b5dcb55198f5883882a39468c3089b5`

## File moves

- Main landmark notebook moved to `notebooks/landmark_pipeline.ipynb`.
- Two earlier notebooks moved into `notebooks/archive/`.
- Application folder renamed from `visualization/` to `demo/`, keeping its scripts, weights, and class mapping together.
- Original report and slides moved to `reports/` with descriptive filenames.
- Report/presentation ZIP contents extracted to `reports/source/report/` and `reports/source/presentation/`.
- Demonstration recording moved to `assets/demo.mkv`.
- Laptop YAML renamed to `configs/laptop_4050.yaml`.

## Documentation and packaging

- Replaced the incomplete root README and empty folder READMEs.
- Documented separate RGB training, landmark notebook, and demo paths.
- Removed unsupported superiority, state-of-the-art, and speed claims from the repository summary.
- Used paired validation metrics from the saved log, with separate report-sourced RGB results.
- Extracted actual existing project figures and the 100-epoch validation CSV.
- Added demo dependencies with a Gradio 5 API compatibility bound.
- Removed the hardcoded CUDA 12.8 extra-index directive from the general requirements file; users select a suitable PyTorch build separately.
- Adjusted ignore rules to keep the supplied demo checkpoint, class mapping, and recording while excluding generated data, weights, caches, and run outputs.

## Preservation and verification

Training and demo Python files, configurations apart from the filename change, notebook bytes, report PDF bytes, demo checkpoint, label mapping, and recording were preserved. No training or application inference was performed. Static verification checks syntax, local Markdown links, YAML/JSON structure, LaTeX image references, and original-file identity. It does not validate runtime dependency compatibility or reproduce model results.

The report and presentation remain original historical deliverables. Team authorship is preserved without assigning unconfirmed individual roles. No repository code license has been introduced.
