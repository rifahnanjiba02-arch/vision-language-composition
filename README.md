# Reproducing MACCO evaluation and a short training run

This repository recreates selected evaluation results for **MACCO**, a model studied for its ability to match images with captions when object relationships, attributes, or word order change. The evaluation notebooks use the authors' released **MACCO CLIP ViT-B/32 checkpoint** to reproduce Tables 1 and 21. A separate notebook records a two-epoch, reduced-scale run of the authors' training code. The short-run model has **not** been evaluated on those benchmarks.

[MACCO paper](https://aclanthology.org/2026.acl-long.1490.pdf) · [Authors' repository](https://github.com/hiker-lw/MACCO) · [Released checkpoint](https://huggingface.co/hiker-lw/MACCO)

## Notebooks and recorded results

| Experiment | Notebook | Results |
| --- | --- | --- |
| Table 1: compositionality benchmarks | [Table 1 evaluation](scripts/MACCO_Table1_Evaluation.ipynb) | [Table 1 CSV](<results/MACCO_Table1 .csv>) |
| Table 21: Winoground and MMVP | [Winoground and MMVP evaluation](scripts/MACCO_Table21_Winoground_MMVP.ipynb) | [Table 21 CSV](results/MACCO_Table21.csv) |
| Short MACCO training run | [Two-epoch training notebook](scripts/MACCO_Train_Short_Run.ipynb) | [Training log](results/out.log) |

The two evaluation notebooks load the released checkpoint, score image–caption matches, and compare our MACCO measurements with the paper. Other model scores displayed in those notebooks are **paper reference values only**. The training notebook starts from pretrained OpenAI CLIP weights, not the authors' trained MACCO checkpoint.

## Datasets used

The notebooks download evaluation data as needed. The [authors' benchmark collection](https://huggingface.co/datasets/hiker-lw/VL-Compositionality-Benchmarks) supplies the Table 1 benchmark files and Winoground data.

| Paper table | Dataset | What the notebook evaluates |
| --- | --- | --- |
| Table 1 | **ARO** | Relation and attribute matching on cropped Visual Genome images; word-order tests on COCO and Flickr images. The notebook obtains additional [COCO Karpathy test images](https://huggingface.co/datasets/undefined443/coco-karpathy-wds) for ARO Order. |
| Table 1 | **Sugar-Crepe** | Captions with altered relationships or attributes. |
| Table 1 | **VL-Checklist** | Action, spatial-relation, and attribute subsets; the notebook extracts only the images required from its large archives. |
| Table 1 | **VALSE** | Actant-swap, action-replacement, and relation subsets. |
| Table 1 | **What's-up** | Controlled-image, COCO, and GQA spatial-matching subsets. |
| Table 21 | **Winoground** | The 171-example clean split used by MACCO, selected from the 400-example dataset in the benchmark collection. |
| Table 21 | **[MMVP-VLM](https://huggingface.co/datasets/MMVP/MMVP_VLM)** | The benchmark's 135 image pairs. |

The datasets above are used for **evaluation**. The short training run used 128 COCO `train2014` image–caption pairs selected from the authors' 109,468-row prepared annotation TSV; the selected images were available in the [COCO train2014 mirror](https://huggingface.co/datasets/ali-sh07/COCO-train2014). Downloaded model weights, images, and archives stay outside Git.

## Results and interpretation

All scores below are percentages. Each benchmark has its own examples and aggregation rule, so a percentage from one row should not be treated as directly comparable with a percentage from another. The linked CSVs contain unrounded scores.

### Table 1: relations, attributes, and word order

| Test | What it checks | Our MACCO | Paper MACCO |
| --- | --- | ---: | ---: |
| ARO Relation | Which objects are related, and how | 73.08 | 73.1 |
| ARO Attribute | Whether a property belongs to the right object | 68.54 | 68.5 |
| ARO Order | Whether changing word order changes the description | 75.92 | 76.0 |
| Sugar-Crepe Relation | A changed relationship in a similar caption | 77.10 | 77.1 |
| Sugar-Crepe Attribute | A changed property in a similar caption | 79.18 | 79.1 |
| VL-Checklist Relation | Whether an action or position matches the image | 70.23 | 70.2 |
| VL-Checklist Attribute | Whether properties such as color or size match | 68.65 | 68.7 |
| VALSE Relation | Whether action and other relationships match | 75.26 | 75.3 |
| What's-up | Whether descriptions of object positions match | 42.67 | 43.2 |

All nine MACCO entries are present. Eight are within **0.1 percentage points** of the published scores; the largest difference is What's-up, at **0.53 points lower**. The [Table 1 CSV](<results/MACCO_Table1 .csv>) gives the exact values and differences.

### Table 21: matching image and caption pairs

Winoground gives the model two images and two captions. Its **text** score requires the right caption for each image, its **image** score requires the right image for each caption, and its **group** score requires both checks to pass. MMVP awards a point only when both matching decisions for an image pair are correct.

| Metric | Our MACCO | Paper MACCO |
| --- | ---: | ---: |
| Winoground text | 32.16 | 32.2 |
| Winoground image | 11.11 | 11.1 |
| Winoground group | 8.19 | 8.2 |
| MMVP average | 21.48 | 21.5 |

The model passes both Winoground checks on **14 of 171 examples** and gets **29 of 135 MMVP pairs** completely right. All four measurements round to the MACCO values reported in the paper. The [Table 21 CSV](results/MACCO_Table21.csv) also includes CLIP and CLIP-FT paper values for context; those models were not run here.

### Short training run: execution check

The [training notebook](scripts/MACCO_Train_Short_Run.ipynb) uses the authors' `main_train_macco_clip.py` and prepared COCO annotations. It keeps the original object, relation, attribute, phrase, and bounding-box columns while selecting the first 128 rows with images available in the mirror. This is a small, nonrandom subset. It reuses the authors' precomputed scene-graph and GroundingDINO annotations instead of running those preprocessing models.

The model starts from OpenAI CLIP ViT-B/32 weights, with both CLIP encoders trainable, plus two text-predictor and three image-predictor cross-attention layers. A local method-binding compatibility fix lets the authors' model code run in Colab. The run retains all four original objectives and their configuration weights:

`total loss = MCA + 0.1 × MIR + 20 × MLM + 250 × MIM`

Here MCA is mask-augmented cross-modal alignment, MIR is mask-augmented intra-modal regularization, MLM reconstructs masked text tokens, and MIM reconstructs masked image patches. No loss function or weight was changed for this run.

| Setting | Short run |
| --- | --- |
| Training pairs | 128 unique COCO images and captions |
| Epochs and batch size | 2 epochs, 16 pairs per batch (8 batches per epoch) |
| Optimizer and precision | AdamW, AMP |
| Learning rates | CLIP: `5e-7`; predictors: `1e-3` |
| Hardware | Tesla T4 GPU |
| Validation | Disabled with `--no-val` |
| Checkpoint setting | Save after each epoch |

The [recorded log](results/out.log) shows both epochs reaching 128/128 training examples and reports these end-of-epoch running averages:

| Training loss | Epoch 1 | Epoch 2 |
| --- | ---: | ---: |
| Total | 478.8 | 425.7 |
| MCA | 1.079 | 0.8828 |
| MIR | 1.464 | 1.389 |
| MLM | 10.48 | 8.666 |
| MIM | 1.072 | 1.005 |

The loss decrease shows progress on this small training subset. It does not establish generalization: no held-out validation or benchmark evaluation was run on the short-run model. This experiment also does not reproduce full-data, five-epoch training or the paper's reported benchmark performance. The log shows that checkpoint saving was configured after each epoch, but no checkpoint is included here or independently verified; the notebook notes that the authors' save routine includes CLIP weights but omits predictor and optimizer state needed for a faithful resume.

## Run the evaluations

1. Open a notebook in Google Colab and run its cells in order. Select a GPU runtime for Table 1; Table 21 supports CPU or GPU.
2. Let the notebook download the checkpoint and required datasets. The notebooks use `open_clip_torch==2.32.0` and Colab's temporary `/content` storage.
3. Review the final comparison and download its CSV before the Colab runtime ends. The Table 1 upload-and-merge cell is optional and is only for resuming from an earlier CSV.

## Revisit the training run

Open the [short-run notebook](scripts/MACCO_Train_Short_Run.ipynb) in a Colab GPU runtime. It fetches the authors' training source and prepared TSV into temporary `/content/macco_run/` storage, configures the reduced run, and applies the method-binding fix. The checked-in notebook is **not yet runnable straight through**: its image-selection cell does not download the 128 selected COCO files, and the example-inspection cell uses `Image` without importing it. Place the selected images under `/content/macco_run/datasets/` at the paths specified by the TSV and import `Image` from PIL before running the remaining cells. The saved notebook has no output for its training or checkpoint-download cells; use the [training log](results/out.log) for the completed run's recorded metrics.

