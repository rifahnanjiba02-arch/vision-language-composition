# MACCO vision-language composition reproduction

This repository is a workspace for reproducing **MACCO-CLIP ViT-B/32** results. CLIP connects images with text. Here, the key question is whether it can tell which caption correctly describes an image's objects, attributes, and relationships.

The [Colab reproduction guide](notebooks/MACCO_Colab_Reproduction_Guide.ipynb) is the step-by-step experiment. It uses the [authors' MACCO code](https://github.com/hiker-lw/MACCO) at commit **97fbae1416e30eba84cc0b8209d9cb6bdedfe6ae**. This repository does not reimplement the model or its evaluators. The notebook clones the official code into **/content/MACCO** during a Colab run and creates documented evaluation copies there.

## Goal and order of work

1. **First milestone: evaluate released weights.** Run Sugar-Crepe with original CLIP and the released MACCO-CLIP ViT-B/32 checkpoint. Check that all images exist, save both full logs, and compare Relation and Attribute scores with the paper.
2. **Extend Table 1 evaluation.** Add ARO, What's-up, VL-Checklist, and VALSE one at a time. Some need extra image collections or manual preparation.
3. **Train later.** A new training run is a separate experiment. The notebook outlines its prerequisites; it does not perform complete training reproduction.

The paper's Sugar-Crepe Table 1 values are **targets**, not measurements produced here:

| Model | Relation | Attribute |
| --- | ---: | ---: |
| Original CLIP | 68.8 | 70.8 |
| Released MACCO-CLIP ViT-B/32 | 77.1 | 79.1 |

The evaluation scripts print accuracy as a fraction: 0.771 means 77.1%. Record measured scores and differences even if they do not match the paper. No benchmark run or reproduced score is claimed here.

## Repository layout

~~~text
.
├── notebooks/
│   └── MACCO_Colab_Reproduction_Guide.ipynb  # Experiment instructions and Colab cells
├── datasets/                                  # Local data layout; contents ignored by Git
│   ├── sugar-crepe/                           # First benchmark
│   ├── ARO/                                   # ARO annotations and Flickr30k images
│   ├── coco/                                  # COCO images needed by ARO Order/training
│   ├── whats_up_data/                         # What's-up benchmark
│   ├── VL_checklist/
│   │   ├── VL_checklist_datasets/
│   │   └── VL_checklist_json_data/
│   └── VALSE/
│       └── probing_data/
├── checkpoints/                               # Model weights; ignored
└── runs/                                      # Logs, environment details, diffs, CSVs; ignored
~~~

These folders are a **local organization template**. The notebook uses explicit Colab paths: extracted benchmarks under **/content/MACCO/datasets/**, the evaluation checkpoint at **/content/MACCO/MACCO-CLIP-ViT-B-32.pt**, and persistent checkpoints and run records under **/content/drive/MyDrive/MACCO_Reproduction/**. Copying data into this repository's datasets folder does not automatically make it available to the notebook.

## Start in Google Colab

1. Open [Google Colab](https://colab.research.google.com/), upload the notebook from **notebooks/**, and save a copy in Drive.
2. Select **Runtime → Change runtime type → GPU**. Run cells in order. Restart the session after package installation as instructed; do not use **Run all**.
3. Let the notebook clone and pin the official repository. Download the **B/32** checkpoint and only the Sugar-Crepe archive for the first milestone. The notebook checks model loading and data paths before scoring.
4. Run the small diagnostic, then both full Sugar-Crepe evaluations. Only the full runs can be compared with the paper.
5. Save the notebook with outputs. Keep each run's environment record, package list, checkpoint hash, dataset counts, source diffs, logs, and comparison CSV together in Drive. Colab's **/content** files can disappear when the session ends.

For later benchmarks, read the matching section of the notebook before downloading data. **ARO Order needs COCO and Flickr30k images; VALSE uses images from Sugar-Crepe and VL-Checklist.** Resolve missing images rather than skipping examples.

## Reproducibility rules

- Record the upstream commit, model and dataset revisions, checkpoint SHA-256, software versions, GPU, seed, and all evaluation script changes.
- Keep upstream files intact. The notebook writes adapted copies and saves their diffs with the run.
- Keep datasets, model weights, generated logs, and local environment files out of Git. The Git ignore rules cover this repository's local folders; Colab and Drive files live separately.
- Label unfinished metrics **not run**. Report released-checkpoint evaluation separately from any model you train yourself.

## Sources

- [MACCO paper](https://aclanthology.org/2026.acl-long.1490.pdf)
- [Official MACCO repository](https://github.com/hiker-lw/MACCO)
- [Released checkpoints](https://huggingface.co/hiker-lw/MACCO/tree/main)
- [Benchmark archives](https://huggingface.co/datasets/hiker-lw/VL-Compositionality-Benchmarks/tree/main)
