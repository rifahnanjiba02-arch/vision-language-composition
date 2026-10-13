# MACCO table reproduction

This project evaluates **original CLIP** and the authors' released **MACCO-CLIP ViT-B/32** model in Google Colab. The goal is to measure their scores on all nine columns of Table 1 in the [MACCO paper](https://aclanthology.org/2026.acl-long.1490.pdf), then compare those measurements with the paper. The same evaluations provide detailed results for Tables 17–20.

**Start with [STEPS.md](STEPS.md).** It gives the order of work, beginning with a Sugar-Crepe setup check and continuing through the remaining benchmarks. The scores shown there are paper targets; this repository has no reproduced scores yet.

## Scope

- Evaluate the released weights for original CLIP and MACCO-CLIP ViT-B/32 on ARO, Sugar-Crepe, VL-Checklist, VALSE, and What's-up.
- Record the available ARO details for Table 17, Sugar-Crepe subsets for Table 18, VL-Checklist Relation and Attribute details for Table 19, and VALSE and What's-up components for Table 20.
- Mark any unfinished metric **not run**. Other Table 1 model rows need their own verified checkpoints. Table 19 Object results are outside the initial scope.
- Do **no training, fine-tuning, Pydantic setup, or extraction of this course repository in Colab**.

## What belongs in this repository

| Path | Purpose |
| --- | --- |
| [STEPS.md](STEPS.md) | Step-by-step Colab and reporting plan |
| notebooks/01_MACCO_Table1_Evaluation.ipynb | Working Colab notebook to save here after creating it; this file does not exist yet |
| results/ | Measured comparison CSVs and relevant full logs; currently empty |

Create **notebooks/** when you save the first notebook from Colab. Existing **configs/**, **scripts/**, and **vendor/** placeholders came from an earlier plan and are not needed for this workflow.

Download and extract the authors' [MACCO source ZIP](https://github.com/hiker-lw/MACCO) **directly in Colab**. Keep its complete evaluation folder, including the bundled OpenCLIP and ARO code. Download the [released B/32 checkpoint](https://huggingface.co/hiker-lw/MACCO/tree/main) and [benchmark datasets](https://huggingface.co/datasets/hiker-lw/VL-Compositionality-Benchmarks/tree/main) there as needed. Keep model files, dataset images, and downloaded archives out of GitHub.

## Expected results

Run the complete Table 1 evaluation for **both** models. Save a comparison CSV with the test name, paper score, measured score, and difference in percentage points. Evaluation code may print fractions: **0.731 means 73.1%**. Preserve the detailed component scores and relevant logs for Tables 17–20, along with the model, data, software, and source details needed to understand each run.

The Colab notebook and reviewed results are the project deliverables. No benchmark has been run or verified in this repository yet.
