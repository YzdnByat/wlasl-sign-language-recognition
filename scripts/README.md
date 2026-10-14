# RGB utility scripts

- `extract_yolo.py`: scans direct `.mp4`/`.avi` files in `./videos`, samples frames, applies a detected-person crop when available, and writes resized frames to `./extracted_frames_yolo`. It does not load the training YAML or accept command-line path flags.
- `create_dummy_data.py`: creates three synthetic frame sequences and a small metadata file. This demonstrates input-file structure only; its split/class coverage is unsuitable for interpreting accuracy.

Run these scripts from the repository root. The [reproduction guide](../docs/reproduction.md) shows a custom-path extraction call and describes incomplete extraction behavior.
