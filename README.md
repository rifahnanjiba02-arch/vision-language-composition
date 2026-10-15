# Reproducing MACCO's vision-language evaluation

This repository reproduces selected **evaluation results** for [MACCO](https://aclanthology.org/2026.acl-long.1490.pdf), a vision-language model tested on whether it understands how objects, attributes, relations, and word order combine in an image and its caption. A model can recognize the individual words or objects yet fail when a small change in their arrangement changes the meaning. The benchmarks here test that distinction through image–caption matching.

We load the authors' [released MACCO CLIP ViT-B/32 checkpoint](https://huggingface.co/hiker-lw/MACCO) and evaluate it on the benchmarks reported in **Tables 1 and 21** of the paper. This reproduces the reported scores using the released model; it does not reproduce model training. Scores for CLIP and other methods shown in the notebooks are copied from the paper for context; only **MACCO (ours)** is measured here.

## What is reproduced

| Paper result | What it tests | Notebook | Recorded scores |
| --- | --- | --- | --- |
| Table 1 | Relations, attributes, and word order across ARO, Sugar-Crepe, VL-Checklist, VALSE, and What's-up | [Table 1 evaluation](scripts/MACCO_Table1_Evaluation.ipynb) | [Table 1 CSV](<results/MACCO_Table1 .csv>) |
| Table 21 | Compositional image–text matching on Winoground and visual discrimination on MMVP | [Winoground and MMVP evaluation](scripts/MACCO_Table21_Winoground_MMVP.ipynb) | [Table 21 CSV](results/MACCO_Table21.csv) |

Both notebooks download the checkpoint and benchmark data, compute image–text similarity scores, and compare the measured MACCO scores with the published values. They use `open_clip_torch==2.32.0` and save their tables as CSV files. Dataset downloads, model weights, and images are kept outside this repository.

## Results at a glance

All scores are percentages; differences below are **our score minus the paper score**, in percentage points. The CSVs contain the unrounded measurements.

For **Table 1**, eight of the nine reproduced MACCO scores are within 0.1 points of the published values. The largest difference is What's-up: **42.67** here versus **43.2** in the paper (−0.53 points). See the [Table 1 CSV](<results/MACCO_Table1 .csv>) for every benchmark and its difference.

| Table 21 metric | Paper MACCO | Our MACCO |
| --- | ---: | ---: |
| Winoground text | 32.2 | 32.16 |
| Winoground image | 11.1 | 11.11 |
| Winoground group | 8.2 | 8.19 |
| MMVP average | 21.5 | 21.48 |

The Table 21 notebook uses the **171-example Winoground clean split** used by MACCO and the **135 MMVP image pairs**. MMVP gives a point only when both decisions for a pair are correct. The [Table 21 CSV](results/MACCO_Table21.csv) also includes the paper's CLIP and CLIP-FT rows as reference values; those models were not evaluated in this repository.

## Run the evaluations

1. Open either notebook in Google Colab and run its cells in order. Use a GPU runtime for the larger Table 1 evaluation; the Table 21 notebook can use CPU or GPU.
2. Allow the notebook to download the [MACCO checkpoint](https://huggingface.co/hiker-lw/MACCO) and its benchmark data. Table 1 uses the [MACCO benchmark collection](https://huggingface.co/datasets/hiker-lw/VL-Compositionality-Benchmarks) and additional COCO images for ARO Order. Table 21 uses that collection for Winoground and the [MMVP-VLM dataset](https://huggingface.co/datasets/MMVP/MMVP_VLM) for MMVP.
3. Review the final comparison table and download the CSV from Colab before its temporary runtime ends. The Table 1 notebook includes an optional upload-and-merge cell for resuming from an earlier CSV.

The notebooks implement their own scoring workflows. The Table 1 workflow has not been independently checked end to end against the authors' bundled evaluator, so measured differences are reported as observed rather than adjusted to match the paper.

**Sources:** [MACCO paper](https://aclanthology.org/2026.acl-long.1490.pdf) · [Released checkpoint](https://huggingface.co/hiker-lw/MACCO) · [Benchmark collection](https://huggingface.co/datasets/hiker-lw/VL-Compositionality-Benchmarks) · [MMVP-VLM](https://huggingface.co/datasets/MMVP/MMVP_VLM)
