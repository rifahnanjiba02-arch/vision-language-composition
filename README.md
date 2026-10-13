# MACCO evaluation project

This repository is for **evaluating released vision-language models**, with no training or fine-tuning. The main goal is to compare original CLIP and the released **MACCO-CLIP ViT-B/32** checkpoint on the nine benchmarks reported in Table 1 of the [MACCO paper](https://aclanthology.org/2026.acl-long.1490.pdf). CLIP matches images with text; these benchmarks test whether it recognizes objects, attributes, relationships, and caption order.

The first practical milestone is Sugar-Crepe. After that, evaluate ARO, VL-Checklist, VALSE, and What's-up. The same runs also produce component results relevant to Tables 17–20. This repository currently contains **structure and documentation only**: no evaluation scripts, configuration, measurements, or reproduced scores have been added.

## What will be compared

These are the paper's Table 1 **reference scores**, not measurements from this project:

| Benchmark | Original CLIP | MACCO-CLIP B/32 |
| --- | ---: | ---: |
| ARO Relation | 58.7 | 73.1 |
| ARO Attribute | 62.7 | 68.5 |
| ARO Order | 54.1 | 76.0 |
| Sugar-Crepe Relation | 68.8 | 77.1 |
| Sugar-Crepe Attribute | 70.8 | 79.1 |
| VL-Checklist Relation | 63.6 | 70.2 |
| VL-Checklist Attribute | 67.7 | 68.7 |
| VALSE Relation | 70.1 | 75.3 |
| What's-up | 41.8 | 43.2 |

Tables 17–20 contain more detailed scores from these evaluations. The first planned report covers the two model rows above and the available detail behind them. Other model rows in Table 1 require their own exact checkpoints and settings; label them **paper reference only** until those are verified. VL-Checklist Object detail is outside this first scope.

## Repository layout

~~~text
.
├── configs/       # Future evaluation settings
├── scripts/       # Future configuration checker and evaluation runner
├── results/       # Measured CSVs, logs, run details, diffs and findings
└── vendor/
    └── MACCO/     # Local copy of the authors' source; ignored by Git
~~~

The folders contain only placeholders for now. The planned Pydantic checker will validate paths, model choice and evaluation settings before a run. Validation alone does not establish that scores reproduce the paper. The planned runner will call the authors' evaluation functions, preserve their source, and save any documented adaptations as diffs.

The authors' source belongs in **vendor/MACCO/** during local or Colab work, including its LICENSE and complete evaluation folders. It is downloaded from the [official MACCO repository](https://github.com/hiker-lw/MACCO) as a ZIP and is ignored by this repository's Git rules. Record the source version or download date with each run. The authors' bundled **eval/open_clip/** is the implementation intended for the initial evaluation.

## Where files go in Colab

| Item | Colab location | Commit here? |
| --- | --- | --- |
| This project | /content/vision-language-composition/ | Project files only |
| Authors' extracted source | /content/vision-language-composition/vendor/MACCO/ | No |
| Extracted benchmarks | /content/macco_data/ | No |
| Released B/32 checkpoint | /content/checkpoints/ | No |
| Persistent run outputs | /content/drive/MyDrive/vision-language-composition/results/ | Copy reviewed results into results/ |

A normal working notebook in Colab can run setup and evaluation commands; this repository does not include a notebook guide. Check that **vendor/MACCO/eval/evaluate_comp_benchmark.py** exists after extraction. ZIP files can add an extra parent folder.

## Workflow

1. Read the [paper](https://aclanthology.org/2026.acl-long.1490.pdf) and the authors' README and evaluation scripts. Prepare a GPU Colab runtime and save run outputs in Drive.
2. Download the authors' source ZIP and the released [MACCO-CLIP ViT-B/32 checkpoint](https://huggingface.co/hiker-lw/MACCO/tree/main). Keep model weights outside Git.
3. Set up an evaluation environment compatible with the active Colab Python version. Record installed versions and fix package errors before downloading benchmarks. Keep the authors' bundled OpenCLIP for the first evaluation.
4. Download [benchmark data](https://huggingface.co/datasets/hiker-lw/VL-Compositionality-Benchmarks/tree/main) **one benchmark at a time**, beginning with Sugar-Crepe. Check annotation files and every referenced image before scoring. ARO Order also needs the referenced COCO and Flickr30k images; VALSE depends on Sugar-Crepe and VL-Checklist image folders.
5. When the project scripts and settings have been added, validate the configuration, then run each benchmark with both original CLIP and MACCO B/32. Preserve full logs and the detailed component scores for Tables 17–20.
6. Compare measured percentages with the paper values. Save the score differences, source changes, package versions, GPU, seed, checkpoint identity, data sources, and a short findings note in **results/**. Mark any unrun metric **not run**.

Evaluation scripts may print accuracies as fractions: **0.731 means 73.1%**. Differences from paper values should be reported in percentage points. A successful setup check or small diagnostic is not a full benchmark result.

## Sources

- [MACCO paper](https://aclanthology.org/2026.acl-long.1490.pdf)
- [Authors' source](https://github.com/hiker-lw/MACCO)
- [Released model files](https://huggingface.co/hiker-lw/MACCO/tree/main)
- [Benchmark files](https://huggingface.co/datasets/hiker-lw/VL-Compositionality-Benchmarks/tree/main)
