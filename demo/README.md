# Local sign-recognition demo

The Gradio app accepts a video and a sentence containing `___`. It extracts MediaPipe pose/hand features, preprocesses them to 516 features per frame, pads or truncates to 40 frames, and applies the included 100-class RobustConvBiLSTM checkpoint. GPT-2 ranks the visual top-ten candidates by loss on the completed sentence.

## Run

From the repository root, with a virtual environment activated:

```bash
python -m pip install -r requirements-demo.txt
cd demo
python app.py
```

Open the address printed in the terminal. Upload or record a short sign video, enter context such as `I wore a ___ on my head.`, and run the decoder. The extraction code rejects sequences with fewer than 15 decoded frames.

## Files

| File | Purpose |
|---|---|
| `app.py` | User interface and pipeline orchestration |
| `extract.py` | MediaPipe model download and landmark extraction |
| `preprocess.py` | Interpolation, spatial normalization, velocities, padding |
| `vision_model.py` | RobustConvBiLSTM definition and visual prediction |
| `llm.py` | GPT-2 sentence-loss ranking |
| `best_convbilstm_model.pth` | Original included visual weights |
| `class_mapping_100.json` | Original 100-label mapping |
| `fix_ffmpeg.py` | Original Windows FFmpeg helper |

The checkpoint and mapping are kept beside `app.py` because the original app resolves them relative to its script. MediaPipe task files are downloaded into the current working directory; launch from this folder. The initial run also needs GPT-2 downloads. Generated task files and caches are not repository inputs.

## Dependencies

`requirements-demo.txt` includes PyTorch, NumPy, pandas, OpenCV, MediaPipe, Gradio, Transformers, and imageio-ffmpeg. Gradio is constrained to the 5.x API because the existing application passes `theme` and `css` to `Blocks`; [Gradio 6 moves those arguments to `launch`](https://gradio.app/guides/gradio-6-migration-guide).

This is a compatibility target inferred from the code, not a recovered lockfile from the original run. The application was not relaunched during this cleanup.

## Interpretation

A webcam recording is processed as a completed clip. Streaming inference, edge-device latency, and translation performance are not measured. The context winner is the candidate with lowest GPT-2 sentence loss; it is not a calibrated combined-model confidence. The UI's agreement/disagreement indicators compare model rankings, not ground-truth correctness.

## Troubleshooting

- Run from `demo/` so task-model paths match the original code.
- Keep the checkpoint and class mapping together. Do not substitute a mapping from another run.
- Confirm network access on first start and allow model downloads to finish.
- Use a video encoding supported by your local decoder; install system FFmpeg if decoding fails. The optional `fix_ffmpeg.py` helper is Windows-specific and is not an automatic cross-platform fix.
- If an environment has Gradio 6 installed, create a fresh environment and install the supplied demo requirements.
