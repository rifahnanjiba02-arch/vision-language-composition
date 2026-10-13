# MACCO Table 1 evaluation

This repository is for checking the **released MACCO-CLIP ViT-B/32 checkpoint** against the MACCO paper. Run its evaluation in **one Google Colab notebook**, measure all nine entries in the MACCO row of Table 1, and keep the detailed scores that also appear in Tables 17–20.

**Begin with [STEPS.md](STEPS.md).** It lists the benchmarks, scoring rules, result records, and where to save each deliverable. The paper values in that plan are targets. **No evaluation has been completed or reproduced score claimed in this repository.**

## Scope

- Evaluate the released **MACCO-CLIP ViT-B/32** model on Sugar-Crepe, ARO, What's-up, VL-Checklist, and VALSE. Start with Sugar-Crepe, then complete all nine Table 1 entries.
- Retain the ARO details for Table 17, Sugar-Crepe subsets for Table 18, VL-Checklist Relation and Attribute details for Table 19, and VALSE and What's-up components for Table 20. Include Object results only if those subsets are actually evaluated.
- Compare measured percentages with the paper values. Record the number of examples, component scores, software and model settings, and any corrections to the evaluator. Mark unfinished tests **not run**.
- Use the [authors' evaluator definitions](https://github.com/hiker-lw/MACCO/tree/main/eval) for preprocessing and scoring. The notebook downloads only the needed evaluation files from a pinned commit.

There is **no training, original-CLIP baseline run, Pydantic setup, or extraction of this course repository into Colab** in the current plan. Other Table 1 model rows require their own exact checkpoints and are outside this evaluation.

## Repository files

| Path | What goes there |
| --- | --- |
| [STEPS.md](STEPS.md) | Complete Colab and reporting plan |
| [scripts/MACCO_Table1_Evaluation.ipynb](scripts/MACCO_Table1_Evaluation.ipynb) | The single Colab notebook; run it and replace it with your Colab copy containing measured outputs |
| results/ | Table 1 comparison CSV, available Tables 17–20 component CSVs, ARO category CSVs, and run settings |

The notebook is an unrun starting point. It does not contain measured results.

Keep the checkpoint, dataset images, and download caches on Colab's temporary disk under **/content**. The notebook writes small results to **/content/macco_results** and creates a ZIP to download. Download it and save your notebook before the Colab runtime ends. Commit the finished notebook and reviewed results to this repository; keep model weights and datasets outside GitHub.

## Result check

For each Table 1 test, record the paper score, measured score, and **measured minus paper** in percentage points. If an evaluator reports a fraction, convert it once: **0.731 = 73.1%**. Do not fill missing measurements with paper targets or use a smaller dataset sample as a full benchmark result.

Sources: [MACCO paper](https://aclanthology.org/2026.acl-long.1490.pdf), [released models](https://huggingface.co/hiker-lw/MACCO/tree/main), and [benchmark datasets](https://huggingface.co/datasets/hiker-lw/VL-Compositionality-Benchmarks/tree/main).
