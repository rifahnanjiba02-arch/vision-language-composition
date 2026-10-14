# MACCO Table 1 evaluation

This repository compares the released **MACCO CLIP ViT-B/32** model with the MACCO row of Table 1 in the [paper](https://aclanthology.org/2026.acl-long.1490.pdf). It uses the trained checkpoint for evaluation; there is no training or run of the other models shown in the paper.

## Files

| File | Purpose |
| --- | --- |
| [Evaluation notebook](scripts/MACCO_Table1_Evaluation.ipynb) | Colab workflow for loading the model, preparing benchmark data, scoring the nine tests, and displaying the comparison |
| [Table 1 results](<results/MACCO_Table1 .csv>) | Recorded MACCO scores and their differences from the paper |

## Recorded Table 1 results

Scores are percentages. **Difference** is our score minus the paper score, in percentage points. The [CSV](<results/MACCO_Table1 .csv>) keeps the unrounded measurements.

| Test | Paper MACCO | Our MACCO | Difference |
| --- | ---: | ---: | ---: |
| ARO Relation | 73.1 | 73.082 | -0.018 |
| ARO Attribute | 68.5 | 68.537 | +0.037 |
| ARO Order | 76.0 | 75.916 | -0.084 |
| Sugar-Crepe Relation | 77.1 | 77.098 | -0.002 |
| Sugar-Crepe Attribute | 79.1 | 79.182 | +0.082 |
| VL-Checklist Relation | 70.2 | 70.230 | +0.030 |
| VL-Checklist Attribute | 68.7 | 68.653 | -0.047 |
| VALSE Relation | 75.3 | 75.262 | -0.038 |
| What's-up | 43.2 | 42.666 | -0.534 |

## Run the notebook

1. Open the [notebook](scripts/MACCO_Table1_Evaluation.ipynb) in Google Colab and select a GPU runtime.
2. Run the evaluation sections in order. The notebook downloads the [released checkpoint](https://huggingface.co/hiker-lw/MACCO) and required [benchmark data](https://huggingface.co/datasets/hiker-lw/VL-Compositionality-Benchmarks) into Colab's temporary `/content` storage. ARO Order uses COCO and Flickr images because they are part of that benchmark.
3. In the final section, use the upload-and-merge cell only when resuming from an earlier CSV. Run the final comparison cell, then download `/content/MACCO_Table1.csv` before the Colab runtime ends. Save the notebook with its outputs and place the reviewed CSV in `results/`.

The notebook uses `open_clip_torch==2.32.0` and its own scoring cells. It has **not** been independently checked end to end against the authors' bundled evaluator, so keep measured differences visible. Other model rows displayed in the notebook are paper reference values only.

Keep model weights, downloaded images, and archives out of GitHub.
