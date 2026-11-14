# Symmetric Recompression Reveals Compression Shortcuts in AI-Generated Art Detection

Code, sample list, and result files for the study of compression shortcuts in AI-generated art detection on the AI-ArtBench benchmark.

The study trains the same ResNet50 detector under three preprocessing conditions that differ only in JPEG compression history, tests each model on every version of the test images, and measures what occlusion maps mark under each condition.

## Main findings

- In AI-ArtBench, human paintings are stored at JPEG quality 95 and generated images at quality 75. File metadata alone identifies the class with 100% accuracy.
- Recompressing only the human class moves the shortcut to the trace of the added encoding (replicated with a second training seed). Recompressing all classes in the same way reduces the dependence.
- Under symmetric recompression, mean recognition of images from an unseen generator over four seeds rises from 1.2% and 2.0% to 21.4% and 10.6%.
- Color is the cue the detector depends on most. A logistic regression on three whole-image color measures transfers across generators better than the detector.
- Occlusion maps pass a deletion test, but maps from two trainings of the same setup agree only in part, and a texture pattern on human paintings found in one training did not replicate.

## Repository layout

```
notebooks/   Google Colab notebooks, in the order they were run
data/        list of the 3,000 sampled images and the fold assignment (no images)
results/     result tables (CSV) and training records (JSON) behind every number in the article
figures/     figures of the article that contain no dataset images
docs/        deviations from the first plan, pre-set reading rules, software environment
```

## Data

The images are not included. They are part of the public AI-ArtBench dataset:

- AI-ArtBench: https://www.kaggle.com/datasets/ravidussilva/real-ai-art (Rammuni Silva et al., 2026, doi: 10.1109/TMM.2026.3743638)
- Its human paintings come from ArtBench-10 (Liao et al., 2022, arXiv:2206.11404)

Notebook 01 downloads the dataset with the Kaggle API and draws the sample with a fixed seed (42). `data/` holds the resulting list, so the sample can be rebuilt exactly.

## How to reproduce

The notebooks were written for Google Colab and read and write a project folder on Google Drive (`MyDrive/Paper2`). Run them in this order. Each notebook saves its outputs to Drive as soon as a part is finished and skips finished parts when it is run again.

| Notebook | What it does | Runtime |
|---|---|---|
| `01_sampling` | Downloads AI-ArtBench, draws 3,000 images (1,500 human, 750 Latent Diffusion, 750 Stable Diffusion; 10 styles), writes the sample list | CPU |
| `02_metadata_normalization` | Reads file metadata, runs the metadata baseline, writes the normalized images and the folds | CPU |
| `03_train_detector` | Trains ResNet50 with 5-fold cross-validation under the original, normalized, and one-sided conditions; cross-generator test | GPU (T4) |
| `04_attribution_maps` | Grad-CAM and occlusion maps, deletion test, model randomization test | GPU (T4) |
| `05_design_measures` | Eleven design measures inside and outside the map regions, with the shuffled control | CPU |
| `06_robustness` | Occlusion on the logit scale, color and blur ablation, design-measure baseline | GPU (T4) |
| `06b_supplementary_cpu` | Bootstrap intervals for the deletion test (one-sided condition) | CPU |
| `07_symmetric_condition` | Symmetric condition: training, compression matrix, maps, deletion test, design measures | GPU (T4) |
| `07b_manuscript_figures_cpu` | Bootstrap intervals for the symmetric condition, map agreement across conditions, Figure 3 | CPU |
| `08_second_seed` | Retrains the symmetric and one-sided conditions with a second seed; maps and design measures | GPU (T4) |
| `09_replication` | Compression matrix with second-seed models; cross-generator test with three further seeds | GPU (T4) |

The notebook text and the column names of the result files are in Indonesian. `results/README.md` gives a glossary and maps each table of the article to its source file.

In the article the three conditions are called normalized, one-sided, and symmetric. In the code and result files they are `normal`, `riwayat_disamakan`, and `simetris`.

## Software

Python 3.13, PyTorch 2.14, torchvision 0.29, NumPy 2.4, SciPy 1.16, scikit-learn 1.6, OpenCV, Pillow 11.3, pandas 2.2, Matplotlib 3.10. Exact versions for each step are in `docs/ENVIRONMENT.md`. The experiments ran on Google Colab with a Tesla T4 GPU.

Grad-CAM and occlusion are implemented directly in PyTorch and were checked against `pytorch-grad-cam` and Captum. The gray-level co-occurrence measures were checked against scikit-image.

## License

The code is released under the MIT License (see `LICENSE`). The dataset images belong to their owners and are governed by the terms of AI-ArtBench and ArtBench-10.

## Citation

If you use this code or these results, please cite the article (details will be added after publication) and the two datasets.
