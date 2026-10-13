1. Read the project goal in **README.md** at the repository root. Read the MACCO paper and inspect the authors' README and evaluation files before running an experiment.

2. Create a working notebook named **MACCO_evaluation_work.ipynb** in **Google Drive/MyDrive/vision-language-composition/**. Use it for Colab commands; it is not a file to add to this repository.

3. Put a copy of this project in Colab at **/content/vision-language-composition/**. Confirm that **README.md**, **configs/**, **scripts/**, and **results/** are in that directory.

4. Download the authors' MACCO source ZIP from their GitHub repository and extract it into **/content/vision-language-composition/vendor/MACCO/**. Keep the authors' **LICENSE** inside that folder. Confirm that **vendor/MACCO/eval/evaluate_comp_benchmark.py**, **vendor/MACCO/eval/evaluate_aro_order.py**, **vendor/MACCO/eval/evaluate_vl_checklist.py**, and **vendor/MACCO/eval/open_clip/** exist. Record the source version or download date.

5. Select a GPU runtime in Colab, mount Drive, and create **/content/drive/MyDrive/vision-language-composition/results/** for persistent outputs.

6. Check Colab's Python and GPU versions, then install the evaluation packages that work with that runtime. Resolve any installation error before proceeding. Restart the session after installation, reconnect Drive, and use the authors' bundled **vendor/MACCO/eval/open_clip/** for the initial evaluation.

7. Download **MACCO-CLIP-ViT-B-32.pt** into **/content/checkpoints/**. Record its source and file hash; keep the checkpoint outside this Git repository.

8. Download **sugar-crepe.tar.gz** first and extract it into **/content/macco_data/**. Inspect the archive layout and confirm the final benchmark folder is **/content/macco_data/sugar-crepe/**. Check the seven subset annotation files and all referenced images, including **val2017/**.

9. Create **scripts/check_config.py** in this repository. Make it validate the source, dataset, checkpoint, results paths, model name, batch size, worker count, and seed using Pydantic.

10. Create **configs/eval.json** in this repository. Set its source directory to **/content/vision-language-composition/vendor/MACCO/**, data directory to **/content/macco_data/**, checkpoint to **/content/checkpoints/MACCO-CLIP-ViT-B-32.pt**, and results directory to **/content/drive/MyDrive/vision-language-composition/results/**.

11. Create **scripts/run_eval.py** in this repository. Make it call the authors' evaluators for original CLIP and released MACCO B/32, save full logs and run details, and save diffs for any evaluation-copy changes. Keep the authors' files in **vendor/MACCO/** intact.

12. Run **scripts/check_config.py** with **configs/eval.json**. Fix missing paths or invalid settings before treating the setup as ready; this check does not produce a benchmark score.

13. Run Sugar-Crepe once with original CLIP and once with MACCO B/32 through **scripts/run_eval.py**. Save each run's logs, metadata, diffs, and comparison CSV under **/content/drive/MyDrive/vision-language-composition/results/**. Check the Relation and Attribute scores against the paper.

14. Add ARO Relation and Attribute annotations and images under **/content/macco_data/ARO/**. For ARO Order, also prepare the referenced COCO images under **/content/macco_data/coco/** and Flickr30k images under **/content/macco_data/ARO/flickr30k-images/**. Check the annotation paths and required language resources, then evaluate both models.

15. Prepare the multipart VL-Checklist data under **/content/macco_data/VL_checklist/**, with **VL_checklist_datasets/** and **VL_checklist_json_data/** beneath it. Check every required image and annotation, then evaluate Relation and Attribute for both models.

16. Extract VALSE annotations under **/content/macco_data/VALSE/probing_data/**. Confirm that its required Sugar-Crepe and VL-Checklist images are present, then evaluate the Table 1 VALSE subsets for both models.

17. Extract What's-up under **/content/macco_data/whats_up_data/**. Check the controlled, COCO-QA, and VG-QA files and images, then evaluate both models and retain all three group results.

18. Create **results/table_1_comparison.csv** in this repository from the measured nine-column results for original CLIP and MACCO B/32. Create **results/table_17_aro.csv**, **results/table_18_sugar_crepe.csv**, **results/table_19_vl_checklist.csv**, and **results/table_20_valse_whats_up.csv** from the detailed logs. Include paper, measured, and difference values; mark unavailable entries **not run**.

19. Copy the reviewed run folders from **/content/drive/MyDrive/vision-language-composition/results/** into this repository's **results/** directory. Keep each run's full logs, environment details, checkpoint identity, source version, data sources, and evaluation-copy diffs. Write a short interpretation in **results/findings.md**.

20. Review **.gitignore** and the files selected for commit. Commit project files and measured results; leave **vendor/MACCO/**, datasets, downloaded archives, checkpoints, and the private planning guide out of Git.
