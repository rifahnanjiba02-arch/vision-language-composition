# MACCO Table Reproduction Plan

**Goal:** use the authors’ trained models to reproduce table results in Colab, then save the notebooks and results to **`vision-language-composition`** on GitHub.

**No training, course-repo extraction, or Pydantic setup is needed.**

## 1. Create a Colab notebook

Name it **`01_MACCO_Table1_Evaluation.ipynb`**.

Select **Runtime → Change runtime type → GPU**.

## 2. Download the authors’ evaluation code

Open [MACCO’s GitHub repository](https://github.com/hiker-lw/MACCO), then download and extract its ZIP directly in Colab.

Keep the complete `eval/` folder, including its bundled `open_clip/` and `ARO/` folders. These contain code the evaluation scripts need.

## 3. Install the required packages

Install the evaluation dependencies and restart the Colab session if necessary.

Confirm that the evaluation scripts import **the authors’ bundled `open_clip` implementation**.

## 4. Download the trained model

Download **`MACCO-CLIP-ViT-B-32.pt`** from the [released model files](https://huggingface.co/hiker-lw/MACCO/tree/main?utm_source=chatgpt.com).

Load it once to confirm that it works before running the tests.

## 5. Download the evaluation datasets

Prepare the five Table 1 benchmarks using the [authors’ dataset collection](https://huggingface.co/datasets/hiker-lw/VL-Compositionality-Benchmarks/tree/main):

- ARO
- Sugar-Crepe
- VL-Checklist
- VALSE
- What’s-up

Download them one at a time. Confirm that the annotation files and referenced images exist. Some tests share images or require additional image folders.

## 6. Adjust the evaluation scripts for Colab

Change the model and dataset paths to your Colab locations.

Use smaller **evaluation** batches if needed. Keep the scoring rules unchanged, and document necessary corrections to the released scripts.

## 7. Run the complete Table 1 evaluation

Run **original CLIP and trained MACCO** across all nine columns:

| Test                   | CLIP target | MACCO target |
| ---------------------- | ----------- | ------------ |
| ARO Relation           | 58.7        | 73.1         |
| ARO Attribute          | 62.7        | 68.5         |
| ARO Order              | 54.1        | 76.0         |
| Sugar-Crepe Relation   | 68.8        | 77.1         |
| Sugar-Crepe Attribute  | 70.8        | 79.1         |
| VL-Checklist Relation  | 63.6        | 70.2         |
| VL-Checklist Attribute | 67.7        | 68.7         |
| VALSE Relation         | 70.1        | 75.3         |
| What’s-up              | 41.8        | 43.2         |

These are the paper’s reference values. Other Table 1 model rows require their exact checkpoints; we will mark those separately until the weights are verified. [aclanthology.org](https://aclanthology.org/2026.acl-long.1490.pdf?utm_source=chatgpt.com)

Start with Sugar-Crepe to check the setup, then finish the remaining benchmarks.

## 8. Record additional tables from the same runs

Keep the detailed outputs so we can compare:

| Table  | Results to record                                  |
| ------ | -------------------------------------------------- |
| **17** | ARO details, including COCO-Order and Flickr-Order |
| **18** | Sugar-Crepe subset scores                          |
| **19** | VL-Checklist Relation and Attribute details        |
| **20** | VALSE and What’s-up component scores               |

These reuse the Table 1 evaluations. Table 19’s Object results require additional evaluation beyond our initial scope. [aclanthology.org](https://aclanthology.org/2026.acl-long.1490.pdf?utm_source=chatgpt.com)

## 9. Compare measured results with the paper

Save a comparison CSV:

| Test         | Paper score | Our score      | Difference   |
| ------------ | ----------- | -------------- | ------------ |
| ARO Relation | 73.1        | Measured value | Ours − paper |

Convert fractions to percentages once: **`0.731 → 73.1`**.

Investigate differences using the model, data, preprocessing and scoring settings. Mark unfinished tests **“not run.”**

## 10. Save to your course repository

From Colab, use **File → Save a copy in GitHub** and choose your repository and intended notebook folder.

Also upload the comparison CSVs and relevant logs to the intended results folder. Keep model files and dataset images outside GitHub.
