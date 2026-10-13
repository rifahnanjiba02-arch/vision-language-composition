# MACCO Table Checking Plan

## Goal

Start fresh in Google Colab. Load the released MACCO model, evaluate it on the required datasets, and compare all nine entries in its Table 1 row with the paper. Keep the detailed scores for Tables 17–20 from the same evaluations.

Use one Colab notebook. No training, cloning, extraction of your course repository, Pydantic setup, or full download of the authors' repository is required. Small evaluation functions must still follow the paper's preprocessing and scoring rules.

Your GitHub repository is `vision-language-composition`. It is the destination for the finished notebook and results, not something you need to load into Colab first.

## Step 1 Create the working notebook

- Create a fresh Colab notebook named `MACCO_Table1_Evaluation.ipynb`.
- Select a GPU runtime.
- Save the notebook so its outputs can be retained as you work.
- Work through one test at a time. Keep the same notebook for all nine results.

## Step 2 Set up only the evaluation dependencies

- Install the packages needed to load the model, read images and annotations, and calculate benchmark scores.
- Restart the session if installation requires it.
- Use a model implementation, tokenizer and evaluation image transform compatible with the released checkpoint and the authors' evaluation settings. A model loading successfully alone does not establish that its settings match.
- Record the Python, PyTorch and model-library versions once. Do not repeatedly change versions to move a score toward the paper.

## Step 3 Load one trained model

- Download `MACCO-CLIP-ViT-B-32.pt` from the official model page.
- Load it as the ViT-B/32 model and put it in evaluation mode.
- Keep the loaded model and its image preprocessing function for every test.
- Do not download B/16, L/14 or SigLIP for the main Table 1 row.
- Do not train or fine-tune the model.

## Step 4 Keep large files off Drive and GitHub

- Keep the checkpoint, download cache and extracted images on Colab's temporary disk under `/content`.
- Save only the notebook, score records and small logs to Drive or GitHub.
- Download one benchmark or required image component at a time.
- Reuse images shared by different tests rather than making duplicate copies.
- Remove a compressed archive when extraction has succeeded and the extracted data are usable. Remember that download caches can also retain archive copies.
- Colab runtime files may disappear after the session ends. Save small results as you go so a reset does not erase the measurements already completed.

## Step 5 Prepare the result record

Keep one record containing the test name, measured percentage, number of evaluated examples, and relevant component scores. Update it after each completed test; build the final comparison table at the end.

These are the paper's targets, not measured results:

| Table 1 test | MACCO B/32 target |
|---|---:|
| ARO Relation | 73.1 |
| ARO Attribute | 68.5 |
| ARO Order | 76.0 |
| Sugar-Crepe Relation | 77.1 |
| Sugar-Crepe Attribute | 79.1 |
| VL-Checklist Relation | 70.2 |
| VL-Checklist Attribute | 68.7 |
| VALSE Relation | 75.3 |
| What's-up | 43.2 |

This plan reproduces the MACCO row. Other Table 1 methods require their own exact trained checkpoints; do not claim those rows were reproduced using MACCO weights.

## Step 6 Run Sugar-Crepe

- Download and extract the Sugar-Crepe benchmark.
- Run Replace Relation to obtain the Relation score.
- Run Add Attribute, Replace Attribute and Swap Attribute.
- Take the unweighted mean of those three Attribute scores for the Table 1 Attribute entry.
- Keep each individual score. Run the additional Object subsets if you want the corresponding detailed Table 18 entries.
- Record the two Table 1 results before moving on.

## Step 7 Run ARO Relation and Attribute

- Download and extract the ARO benchmark, including its annotations and images.
- Evaluate Relation using the authors' category exclusions and category-level average.
- Evaluate Attribute using the benchmark evaluator's category eligibility rule, which excludes categories with fewer than 25 examples, and category-level average.
- Preserve the authors' rounding and scoring procedure. Do not substitute a single pooled accuracy over all examples.
- Record both results.

## Step 8 Run ARO Order

- Prepare the COCO-Order and Flickr-Order annotations and their referenced images.
- Obtain missing COCO or Flickr images separately if the ARO archive does not contain them. Use the referenced split and filenames.
- Install the language resources needed to generate the shuffled captions.
- Fix and record the random seed for these caption permutations.
- Run COCO-Order and Flickr-Order separately.
- Average their two scores for the Table 1 ARO Order entry.
- Keep both component scores for Table 17. The paper's MACCO values are 72.3 and 79.6, averaging to 76.0 after rounding.

## Step 9 Run What's-up

- Download the What's-up data, including controlled images, COCO-spatial and GQA-spatial data.
- Evaluate both subsets within each group, following the intended benchmark procedure.
- Use the VG-QA dataset for both VG/GQA subsets. Document the correction to the released script's second VG-QA call, which uses COCO-QA by mistake.
- Calculate each group's two-subset mean, then the equal-weight mean of the three groups for Table 1.
- Keep all component scores for Table 20. The paper's group values are 34.2, 47.1 and 48.3; their mean is 43.2.

## Step 10 Run VL-Checklist

- Prepare the annotations and images required for Relation and Attribute only.
- Reuse existing Visual Genome images where the annotation paths refer to the same files.
- Avoid downloading the entire benchmark collection. If the needed images are only available in large archives, process the required components individually and allow enough space for extraction.
- Run the HAKE, SWIG and Visual Genome action-relation subsets, plus the Visual Genome spatial-relation subset.
- Average the three action scores, then average that action mean with the spatial score for the Table 1 Relation entry.
- Run the VAW and Visual Genome Attribute subsets. Average the two source scores for each attribute type, then average the five types: action, colour, material, size and state.
- Record the two Table 1 entries and keep the detailed scores for Table 19. Object columns are outside this initial scope.
- If the required complete data do not fit, mark this test blocked by storage. A smaller sample is not a reproduction of the full benchmark.

## Step 11 Run VALSE

- Download the VALSE annotations needed for the reported Relation aggregate.
- Reuse the required action images from VL-Checklist and spatial-relation images from Sugar-Crepe.
- Evaluate Actant Swap and Action Replacement; average them to obtain the Action score.
- Evaluate the spatial Relations subset.
- Average the Action and spatial Relation scores for the Table 1 VALSE entry.
- Keep the two components for Table 20. The paper's MACCO values are 78.6 and 72.0, averaging to 75.3.

## Step 12 Build the complete comparison table

- Combine all nine measured percentages with their paper targets.
- Add a difference column: our score minus the paper's score, in percentage points.
- Convert accuracy fractions to percentages once. For example, 0.731 corresponds to 73.1.
- Keep unfinished entries marked `not run`; do not fill them with the expected values.
- Do not replace measured values with the paper's numbers or round them selectively to make them agree.
- Investigate differences using the model settings, complete test split, image handling and score aggregation. The paper does not prescribe a universal agreement threshold for released-checkpoint evaluation.

## Step 13 Make the additional comparison tables

Use the detailed outputs already collected:

| Paper table | Results to compare |
|---|---|
| 17 | ARO Relation, Attribute, COCO-Order and Flickr-Order |
| 18 | Sugar-Crepe subset scores; include Object entries only if evaluated |
| 19 | VL-Checklist Relation and Attribute details; label Object columns not run |
| 20 | VALSE Action/Relation and the three What's-up group scores |

These reproduce the available MACCO entries, not every competing-model row in those tables. This is a suggested additional scope because it reuses the Table 1 runs; it is not a statement that Sir specifically required these table numbers.

Do not add training-dependent ablation or probing experiments to this evaluation-only task.

## Step 14 Save to your existing course repository

| Repository folder | Deliverable |
|---|---|
| `scripts/` | `MACCO_Table1_Evaluation.ipynb`, including the measured outputs |
| `results/` | Table 1 comparison CSV, additional comparison CSVs, small logs and run settings |
| `configs/` | Optional settings file; it can stay empty for this notebook workflow |

- Save the notebook from Colab to the intended GitHub folder when ready.
- Upload the small result files to `results/`.
- Keep datasets and checkpoint files outside GitHub.
- Include the checkpoint filename, dataset sources, software versions, random seed and any evaluator corrections in the experiment record.

## Sources

- [MACCO paper](https://aclanthology.org/2026.acl-long.1490.pdf)
- [Released trained models](https://huggingface.co/hiker-lw/MACCO/tree/main)
- [Benchmark data](https://huggingface.co/datasets/hiker-lw/VL-Compositionality-Benchmarks/tree/main)
- [Authors' evaluator definitions](https://github.com/hiker-lw/MACCO/tree/main/eval)

The evaluator definitions are references for matching the paper's protocol. This plan does not require downloading the entire authors' repository.
