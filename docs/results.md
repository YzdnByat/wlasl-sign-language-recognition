# Results and provenance

## Source of the landmark metrics

`notebooks/landmark_pipeline.ipynb` is the original `Sequence_Models_NEW.ipynb`, moved without changing its bytes. Its zero-based code cell 18 defines `RobustConvBiLSTM`, runs 100 epochs, and prints training accuracy, validation Top-1, and validation Top-5. It saves a checkpoint when validation Top-1 strictly increases.

| Quantity | Value | Epoch | Meaning |
|---|---:|---:|---|
| First attainment of the best recorded validation Top-1 | 43.56% | 63 | First best-Top-1 checkpoint selection at this level |
| Top-5 at that same epoch | 68.32% | 63 | Paired validation metric |
| Highest recorded validation Top-5 | 72.28% | 61 | Independent Top-5 peak |
| Top-1 at that Top-5 peak | 35.64% | 61 | Paired validation metric |

Subsequent epochs repeat 43.56% Top-1, with other Top-5 values. The code uses `>` rather than `>=`, so a tie does not overwrite the previously selected checkpoint.

The final cell of the notebook is empty. The RobustConvBiLSTM cell defines a test loader but does not evaluate it after training. Therefore the saved 43.56% / 72.28% figures must not be described as a paired held-out test result.

The included demo checkpoint was preserved, but no run manifest proves it is the exact file saved at epoch 63. The README explains the notebook's selection rule, without asserting a verified checkpoint-to-run association.

`results/robust-convbilstm-validation.csv` contains the 100 logged epoch rows parsed from the original notebook's saved output. No model was rerun to produce this CSV.

## Split and class selection

The main notebook selects the 100 glosses with the most metadata instances. Its `GroupShuffleSplit` operations use base video ID to create approximately 80% training, 10% validation, and 10% test groups with seed 42. That grouping keeps versions sharing a base video ID together; it does not group by signer. Small classes may not be represented in every split.

The RGB loader uses official metadata `split` labels, filtered to extracted video folders. Its class mapping is built from the supplied metadata. These experiments therefore differ in both class count and split construction.

## Historical report figures

The original report and presentation list 23.94% Top-1 / 59.81% Top-5 as a Swin3D-T test result for the 2,000-class experiment. They also place the same numbers in a 100-class comparison. The original standalone Swin run logs and evaluated sample IDs are not supplied, so the README treats those numbers as report-sourced results and does not repeat the cross-subset improvement claim.

The report's Pose-TGCN table lists 12.87% / 33.66%. The notebook's saved graph-model output stops after epoch 40, reaching 16.83% validation Top-1 at that point; the code contains a later test-evaluation block, but no corresponding final test output is saved. The summary therefore does not present the report's graph figures as an independently verified test pair.

The original PDFs and LaTeX sources remain historical deliverables. The root README and this provenance note specify the interpretation used for the repository's current summary.

## Demo interpretation

The application ranks the visual classifier's top ten candidates by GPT-2 loss on the completed context sentence. It does not combine that loss with visual confidence into a calibrated joint probability. The screenshot and recording illustrate operation; they do not establish a measured improvement from context reranking.

Any future benchmark should retain sample IDs, class mapping, split definitions, run configuration, checkpoint identity, and test predictions for that exact run.
